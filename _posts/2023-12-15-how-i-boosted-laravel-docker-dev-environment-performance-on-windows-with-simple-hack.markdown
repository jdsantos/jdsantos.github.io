---
title: "Docker + Windows for Laravel Development: How I boosted performance with just this simple hack"
layout: post
date: 2023-12-15 20:00
image: /assets/images/posts/20231215/1.png
headerImage: true
tag:
- docker
- laravel
- performance
- development
- windows
star: false
category: blog
author: jdsantos
description: A simple hack to boost your Laravel Docker dev environment performance on Windows
---

Hi there! It's Jorge. In this post, I'm going to reveal a simple hack you can make on your docker-compose file if you're developing a Laravel project on a Windows machine with Docker, and experiencing **really slow disk performance**.

This was the docker-compose.yml file used in the development:

```
version: "2.3"
services:
  app:
    build: ./app
    container_name: app
    environment:
      PHP_OPCACHE_ENABLE: 0
      PRODUCTION: 0
    ports:
      - "8080:80"
    depends_on:
      - "database"
      - "redis"
    volumes:
      - ./app:/app
```

Notice the volume `./app:/app`. This volume allows us to bind our local directory `app` to the container's /app directory so that when we change the code in the host machine, that code changes immediately on the container so that we can run and test our changes.

## 😈 vendor directory - the root of all evil

When trying to figure out why the hell was this app so slow in local development environment vs the deployed production environment, I started to monitor my machine's resources at each step of the request. I noticed that serving static resources with nginx was always fast, but when the PHP backend kicked in, my SSD disk usage skyrocketed.

I started to dig in the Laravel PHP backend, and noticed that even running a simple command like `composer dump-autoload` was extremely slow; so my focus was now on optimizing this step alone. Again, started to notice that my container was heavily using my SSD just for the autoload generation. 

In my research on this problem, I bumped [this post](https://code.visualstudio.com/remote/advancedcontainers/improve-performance) about how to improve performance on VS Code DevContainers (which I was already trying as well). This explanation by Microsoft said it all:

> *Since macOS and Windows run containers in a VM, "bind" mounts are not as fast as using the container's filesystem directly. Fortunately, Docker has the concept of a local "named volume" that can act like the container's filesystem but survives container rebuilds. This makes it ideal for storing package folders like node_modules, data folders, or output folders like build where write performance is critical.*

I then changed my docker-compose.yml file to include a named volume specifically for the `/vendor` directory but kept the original bind mount as well. After running `docker-compose up -d` again, running `composer dump-autoload` got really **fast**! The problem was solved. 😌

Here is what my final docker-compose.yml looked like:

```
version: "2.3"
services:
  app:
    build: ./app
    container_name: app
    environment:
      PHP_OPCACHE_ENABLE: 0
      PRODUCTION: 0
    ports:
      - "8080:80"
    depends_on:
      - "database"
      - "redis"
    volumes:
      - ./app:/app
# Add this vendor named volume for disk read/write performance boost
      - vendor-dir:/app/vendor
volumes:
# Don't forget to add it to the volumes section!
  vendor-dir:
```

## 💻 Environment

This was the development environment that I used for this legacy project:

 Hardware
 - Intel i7-8750H
 - 24 GB of RAM
 - SSD 250GB

Software
 - Windows 11 Enterprise
 - Docker Desktop with WSL backend
 - Visual Studio Code

Hope this helped you in any way.

Hope to see you soon 👋
