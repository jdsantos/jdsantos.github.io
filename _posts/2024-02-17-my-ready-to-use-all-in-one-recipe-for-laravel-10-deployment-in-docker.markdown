---
title: "Laravel 10 + NGINX + PHP-FPM - my ready to use all in one recipe for your Laravel 10 deployment in Docker"
layout: post
date: 2024-02-17 20:00
image: /assets/images/posts/20240217/1.png
headerImage: true
tag:
    - docker
    - laravel
    - opcache
    - php
    - phpfpm
    - nginx
star: false
category: blog
author: jdsantos
description: An easy to use and production ready configuration for your Laravel project with Alpine + NGINX + PHP-FPM + OPCache 
---

Hi there! It's Jorge. Is it just me, or did January felt like it had 90 days or so? Well, nevermind. Let's dive right in on what brought you here.

In this post, I'm continuing on the Docker + Laravel theme but now on a diferent topic: **my deployment strategy for either on premise or cloud Laravel apps using containerization**.

If you are on a hurry, you can find this on my github repo [LANPOD - Laravel + Alpine + Nginx + PHP-FPM + OPCache + Docker](https://github.com/jdsantos/laravel-alpine-nginx-phpfpm-opcache-docker) and on your way there please  leave a star on this repo ⭐! Thanks in advance!


## 🤯 but why? why when there are so many options?

You're absolutely right. There are a lot of options out there with ready to use base images, **lots and lots** of configurations out of the box, but at least to me they all of them were a bit too much: either they packed a lot of stuff I didn't need, or they required lots of tunning to be able to run my apps accordingly to my clients requirements. Some of them were really heavy weight images 🥊, and extending them was also hard.

That is why I prefered to build these small docker images, that pack the absolute minimum to run my Laravel apps, using the proven php-fpm on a base lightweight Alpine image, merged together with NGINX so that you can make the configuration an absolute breeze. I also packed supervisor and opcache that I usually use in my deployments.

![Tech stack](/assets/images/posts/20240217/1.png)

## ☝️ first things first

First of all, I started by inheriting from the base alpine php-fpm:8.3 base image and installed opcache + nginx + supervisor:

```
FROM php:8.3-fpm-alpine

RUN apk --no-cache add \
    nginx \
    supervisor \
    && docker-php-ext-enable opcache

```

## 🦸 supervisor: a single process to rule them all

Then, as my apps usually need to process Laravel queues, I included supervisor to be able to run (and keep running at all times) the worker processes. Docker is really good at maintaining and restarting a single entrypoint process. However, if you launch sub processes within your app like supervisor, you must ensure it starts with things like `service start supervisor`. Then it hit me: what if I used supervisor as my main docker process and entrypoint, and not only to run my queues? Then supervisor could handle and monitor all sub processes I need on my image on its own!

I went for it, and in the end this is how my entrypoint exec command looks:

```

# Start supervisord
exec /usr/bin/supervisord -c /etc/supervisord.conf

```

This instruction launches supervisor that spawns correctly these sub processes:

```
[program:nginx]
command=/usr/sbin/nginx -g "daemon off;"

[program:php-fpm]
command=/usr/local/sbin/php-fpm

[program:crond]
command=/usr/sbin/crond -f

[program:queue-worker]
command=php /opt/laravel/artisan queue:work --sleep=3 --tries=3 --backoff=3 --max-time=3600

```

## ✨ nginx: my frontend server of choice

nginx is my choice in terms of serving content and handling incoming requests. The configuration is also very simple, and with these few lines, I got it working just fine:

```

        try_files $uri =404;
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_pass localhost:9000;
        fastcgi_index index.php;
        include fastcgi_params;
        # Block httpoxy attacks. See https://httpoxy.org/.
        fastcgi_param HTTP_PROXY "";
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_param PATH_INFO $fastcgi_path_info;

        # FastCGI buffering settings
        fastcgi_buffering on;
        fastcgi_buffers 16 32k;
        fastcgi_buffer_size 64k;
        fastcgi_busy_buffers_size 128k;
        fastcgi_temp_file_write_size 128k;
    
```

## ⚡ php-fpm: a good php process manager

As far as handling requests to my Laravel php backend, I prefer to use FastCGI Process Manager that manages PHP processes nicely. With just this configuration file, everything is looking good:

```
[www]
listen = 9000
listen.allowed_clients = 127.0.0.1
user = www-data
group = www-data
pm = dynamic
pm.max_children = 5
pm.start_servers = 2
pm.min_spare_servers = 1
pm.max_spare_servers = 3
```

> Notice that although the 127.0.0.1 ip address seems harcoded, it works fine as the nginx + php-fpm run inside of the same container (no need for complicated networking here...)


## 🔥 opcache: my frontend server of choice

In production environments you should use OPCache so that PHP doesn't need to load up and parse your PHP scripts on each request.

Therefore, I configured my instance as follows:

```
[opcache]

opcache.enable=${PHP_OPCACHE_ENABLE}
opcache.revalidate_freq=0
opcache.validate_timestamps=1
opcache.max_accelerated_files=10000
opcache.memory_consumption=192
opcache.max_wasted_percentage=10
opcache.interned_strings_buffer=16
opcache.fast_shutdown=1
``` 

## 

You can find this on my github repo [LANPOD - Laravel + Alpine + Nginx + PHP-FPM + OPCache + Docker](https://github.com/jdsantos/laravel-alpine-nginx-phpfpm-opcache-docker) and on your way there please  leave a star on this repo ⭐! Thanks in advance!

## 💻 Environment

This was the environment that I used:

Hardware

-   Intel i7-8750H
-   24 GB of RAM
-   SSD 250GB

Software

-   Windows 11 Enterprise
-   Docker Desktop with WSL backend
-   Visual Studio Code

Hope this helped you in any way.

See you soon! 👋
