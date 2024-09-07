---
title: "Get your Laravel app ready for deployment in Docker with just these two commands"
layout: post
date: 2024-09-06 20:00
image: /assets/images/posts/20240906/1.png
headerImage: true
tag:
    - docker
    - laradocker
    - laravel
    - php
    - nginx
star: false
category: blog
author: jdsantos
description: Get your app to work on Docker (or Podman) with just a couple of console commands. 
---

Hi there! It's Jorge. It's been a while since my last post, and this one might sound like the same as [one that you might have read around here](https://jdsantos.github.io/my-ready-to-use-all-in-one-recipe-for-laravel-10-deployment-in-docker). But come along and I promise you that never again will have to build docker images from scratch to your [Laravel](https://laravel.com) projects!

## ⚡TL;DR

In case you are in a hurry, [this is the link of the Github repository for the Laradocker project](https://github.com/jdsantos/laradocker), a brand new composer package that sets up your Laravel project with Docker with just these two simple commands:


```bash

# Run these commands inside your Laravel project:

composer require --dev jdsantos/laradocker

php artisan laradocker:install

# That's it! Follow the instructions and you are done!

```



## 📜The backstory

In my [previous post](https://jdsantos.github.io/my-ready-to-use-all-in-one-recipe-for-laravel-10-deployment-in-docker), I brought you [LANPOD](https://github.com/jdsantos) a Laravel/Docker deployment strategy for that allows you to deliver a Laravel app on premise or cloud environments relying on a battle tested tech recipe consisting of **laravel + alpine linux + nginx + php-fpm + opcache + docker** you absolutely loved.

This recipe allows you to deploy your app in a redistributable, virtualized, os agnostic, self-contained and self-configured software image and run it in virtualization engines such as [Docker](https://docker.com) or [Podman](https://podman.io). It even includes things out of the box like supervisor's tidy configuration for handling your queues, nice defaults for php, opcache and php-fpm, nginx, etc.

**All good, but...**

## 😵The pain

**Something was off.**

While LANPOD as a template repository on Github was a helpful starting point to a brand new Laravel app, and a **LOT** better that having to go about wiring virtualization over and over again on the same structured apps, it was completetly useless to an already existing/legacy project. You would have to **MANUALLY** extract the recipe's files and directories of the bunch of other stuff, and adapt them to your needs. As it was meant to be a template only repository, it didn't include right out of the box any native database support (because if it did, it would have to include all of them in the same image), and if an update to the configuration was needed, it would be hell on earth just to know how and what to change in your project.

## 💊The solution

Having felt this pain myself enough times already, especially when having to add by hand the database dependencies to the Dockerfile/Containerfile over and over again, I realized: how cool would it be to have this recipe as a dependency in my own projects, in such a way that is easy peasy to install, uninstall and update in the future?


<div style="text-align:center">

> **"That's it! I'm going to create a Laravel package that I can install in my projects! Let's do this!"**

![Adventure gif](/assets/images/posts/20240906/2.gif)

</div>

So, the adventure began!

I started to scaffold my brand new composer project with all required dependencies to develop and test Laravel packages including `orchestra/testbench`, `laravel/pint` and `larastan/larastan`. 

After that, I set up right from the start a [Github repository](https://github.com/jdsantos/laradocker) for the project, auto-publish capabilities to [Packagist](https://packagist.org/packages/jdsantos/laradocker), the PHP composer package repository, and spawn Github actions for a super simple CI/CD pipeline to run my tests at each push.

With all this in place, the rest was peanuts: I used all of [LANPOD](https://github.com/jdsantos/laravel-alpine-nginx-phpfpm-opcache-docker)'s files as mere stubs for this project, and used the plain [Artisan Console](https://laravel.com/docs/11.x/artisan) to develop a pretty silly and basic UI for installing this recipe:

![Terminal UI](/assets/images/posts/20240906/3.png)

After launching Laradocker inside your project, the installer will guide you through the necessary steps to build the proper dependencies in the image, such as database connectivity support and you are done! 

All files get generated in an instant and copied into your project!

## The code

This package is completely free & open-source and **[you can find the source code here!](https://github.com/jdsantos/laradocker)**. On your way there, please leave a star ⭐ on the repo to show your support. 

Thanks in advance ❤️

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
