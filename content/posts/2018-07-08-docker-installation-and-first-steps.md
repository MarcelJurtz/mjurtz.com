---
title: 'Docker - Installation and First Steps'
date: 2018-07-08T06:00:00+00:00
author: Marcel Jurtz
layout: post
permalink: /2018/07/docker-installation-and-first-steps
categories:
  - development
tags:
  - development
  - software
  - docker
  - containers
  - programming
---

After outlining the theory behind container management with Docker in last week's article, I would like to continue the topic in a more hands-on way today. 

I'll show you the installation and the first steps in Docker. Let's get started! 

## Installation

Docker is available for Windows, Linux and Mac OS. For the download you'll need to sign up for a free account.

I would like to point out that the Windows version is not intended for production use, only for development and testing. It runs (currently) on a Mobylinux Hypervisor VM, but a native Win10 application is under development. Nevertheless, you will hardly notice this, the usage feels native and you can work on Windows with a command line of your choice.

The first command I want to show you is docker info. This will show you information about your installation. If you are using Windows, you can see here what I just mentioned: The client specifies Windows as OS, but the server indicates Linux.

## Running Your First Container

Next we want to create and start our first container. We use the following command for this:  

```bash 
docker run hello-world 
```

Docker run is the command to start a container, followed by the name of the image, in this case hello-world (Tip: If you get a timeout under Windows, try to set the DNS server to fixed in Docker's network settings, this helped me).

You will often stumble across the term docker engine. The Docker Engine includes the Docker Client and the Docker Daemon. The client makes API calls to the daemon, which then processes the commands. In my example, the Docker Daemon now looks for an image named hello-world. If this is not found locally, the search is continued in the default registry, usually the docker hub. The image is downloaded (if one is found) from there and started. When a container is stopped, the local copy of the image is retained, so that a later call of it does not require a new download.

The container has now been started. You can view all active containers with the command 

```bash 
docker ps 
```

With the flag -a you can specify that stopped containers should also be displayed. 

You can also download an image without starting a container immediately: 

```bash 
docker pull hello-world 
```
 
I generally recommend that you always specify the version of the image you want to load. You can enter the version number separated by a colon after the name of the image. Example: 

```bash 
docker pull hello-world:1.2 
```
 
To remove an image, use the command 

```bash 
docker rmi hello-world 
```

The version number can also be specified here; this is done in the same style as for the pull.

## Creating Your Own Images

Now I want to create my own image. I want to write a simple PHP hello world application. For this I need a so called dockerfile. These files describe images and serve as a kind of blueprint. For the realization of a PHP application PHP and a compatible web server is required. I will use Apache here, so I need an image with Apache, PHP and my own script. First I need a suitable docker file. Fortunately, I don't have to write it myself, I can use templates from the Hub.

On the Docker Hub I can find it by searching for'PHP' and using one of the entries that lists the keyword'Apache'. You will find here different entries, each with a highly specialized version on the left, to the right the versions become more and more general. I recommend that you use one of the more specialized versions to prevent unwanted updates.

I decide to use the image with PHP 7.2 and Apache Stretch. The Hub page lists how to handle the template:

```bash 
FROM php:7.0-apache 
COPY src/ /var/www/html/  
```

Since I want to use PHP 7.2, I update the version number accordingly. This script now corresponds to the content of my docker file. Brief summary: I now have a folder containing my docker file with the above content and another folder 'src' containing the index.php with a simple Hello-World script: 

```php 
<?php 
echo 'Hello Docker!'; 
?> 
```

Docker images are 'layered'. The PHP image is based on other images and so on. Our own image is based on php:7.2-apache. This allows us to keep our docker file short and concise and to include PHP and Apache with just a single line. So our docker file will download the PHP image from Docker Hub, and copy the files from the src folder to the /var/www/html/ directory within the container. 

Now the image must be built. To do this, I use the command 

```bash 
docker build -t hello-docker . 
```
 
Hello-docker is the name of my image, the -t flag is used to assign a name. At the end you have to specify the path to the docker file. The point references the current directory. 

After the image has been created, it can be started in the form of a new container.  

```bash 
docker run -p 80:80 hello-docker 
```

The call looks almost like the one we've already met. The -p 80:80 addition merely defines that the traffic arriving at port 80 on the host is forwarded to port 80 of the container. 

After the start you can now reach the web server via localhost. Your first own container is now up and running!