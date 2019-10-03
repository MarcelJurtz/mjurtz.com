---
title: 'Dockerizing .NET Core Apps'
date: 2018-08-19T06:00:00+00:00
author: Marcel Jurtz
layout: post
permalink: /2018/08/dockerizing-net-core
categories:
  - development
tags:
  - development
  - software
  - docker
  - containers
  - programming
---

I have already written two articles about the basics of Docker. 
While these should provide a general overview of you technology, I would like to illustrate a concrete application example today. 
I will show you the whole process, from writing a simple application to using it in the form of a Docker Container.

If you haven't read my previous articles, you may not have the idea why you should use Docker at all. 
For a full explanation, I refer to my other articles which you can check out 
[here]({{site.baseurl }}{% post_url 2018-07-01-docker-getting-started %}) and 
[here]({{site.baseurl }}{% post_url 2018-07-08-docker-installation-and-first-steps %}). 
However, I would still like to comment briefly on the idea. Imagine you are writing a.NET application. 
To use your application you now need a special version of the.NET framework, 
as well as possibly a suitable database and other dependencies.

For larger applications it may well be that just installing and setting up the environment is not a trivial task. 
Over the years, this concept has become simpler, partly through the use of VMs and installation. 
Nevertheless, according to this approach, an application requires a complete VM, which is not really resource-saving.

Docker allows you to map all dependencies in the form of a docker image. 
This image can later be instantiated as a container, which forms a self-contained, isolated unit. 
You can imagine a container like a VM, but the Docker container does not have its own operating system. 
Starting and stopping a container does not require a complete startup and shutdown of an operating system, 
which significantly reduces the loading times.

So far so good. Please install Docker first, if you haven't already done so. You will find instructions on how to do this 
[here]({{site.baseurl }}{% post_url 2018-07-08-docker-installation-and-first-steps %}).

You have now successfully installed Docker and are ready to go. Before we write our own images, 
I would like to go into the general usage. Docker is available from the command line. 
So on Windows you can use the CMD, Powershell or a counterpart of a third party.

Docker commands are initiated with the keyword docker. If only this is entered without a subsequent command, 
a list of available commands is displayed.

*Docker version* gives you information about the installed version of Docker. 
You see two entries here,a server and a client version. The server version will mention a Linux-OS. 
During installation you could choose between Linux and Windows containers. 
The Linux containers are selected by default, in this case the containers use the Linux kernel of the host operating system. 
If you are using Windows, Docker will create a minimal Linux VM to serve as host. 
You can see this when you open the HyperV Manager, there you will see an entry with the name MobyLinuxVM.

You can also change your choice of container technology later by selecting the Docker icon in the system tray. 
The context menu that appears contains an entry for switching from Linux to Windows (and vice versa). 
However, Linux containers are the usual variant and I recommend that you use them, unless you have a special reason for changing.

The *docker info* command lists information about the installed images and instantiated containers. 
This list is currently empty because Docker has just been installed. 
The *docker run hello-world* command starts the image named hello-world. The procedure here is as follows: 
	
* The Docker Client first contacts the Docker Engine (also called Docker Daemon), which determines that the image is not locally available. 
* It then searches the Docker Hub (a platform for exchanging Docker images) and downloads the appropriate image. 
* The engine then creates a container containing an application that outputs "Hello World".
* This output is sent to the Docker Client, which outputs the text in the console.

Downloaded images are kept locally. If you run it again, no new download takes place, the local image is reinstantiated. 
Images can also be downloaded without being executed immediately. Use the command *docker pull imagename* to do so.

With the command *docker ps* you can view the running containers. 
Here, however, you must note that some containers are started, fulfill a task and are then ended again. 
Other containers run passively in the background and are not terminated automatically. 
Examples are web servers or database management systems.
To also display paused and stopped containers, use the flag -a. 
You can in general interact directly with specific containers. 
The basis for this is always the container ID, an alphanumeric character string with which the container can be uniquely identified. 
However, you do not have to enter the entire ID, the first digits are sufficient until no more mix-ups are possible.

Images are usually tagged. Tags are used to specify a version of an image. 
If no tag is specified, the latest version is used by default. 
However, this is not recommended, as it can lead to problems with later releases. 
So always try to use a specify a version of images you use. 
To do that, simply add a colon, followed by the version after the name of the image.

I think that's enough for now with the basics. Now we will develop our own application with Docker support. 
I am using Visual Studio Code, so the approach will work on all major operating systems.

With the command *dotnet new console* I instantiate a new console application. 
The template creates a Hello-World application, which can be started with the command *dotnet run*. 
Next, we'll take care of linking to Docker.

First, a dockerfile must be created. Dockerfiles describe the structure of an image and are usually based on other images. 
My application is based on the *microsoft/dotnet:2.1-sdk* image. 
The dockerfile is created without a file extension with the name dockerfile on csproj file level and it contains the following content:

```
FROM microsoft/dotnet:2.1-sdk

WORKDIR /app
COPY /bin/debug/netcoreapp2.1/ .

ENTRYPOINT ["dotnet","NET_CORE_Docker.dll"]
```

NET_CORE_Docker is the name of the project. Now an image must be built based on the docker file. 
This can be done with the command *docker built -t net_core_docker .*.

The dot at the end defines the current directory as the starting point. 
Net_core_docker is the name of the image, only lowercase letters are allowed.

The image can now be instantiated with *docker run net_core_docker* and the container will be started automatically. 
As expected, the output is *Hello World!* Our first project is running!

Of course you can also work with Visual Studio instead of VS Code. 
Docker support is already implemented here and you can add it to your project by simply right-clicking on the project 
and selecting *Add Docker Support*. The docker file is then created automatically. 

Our first.NET core application now runs with Docker. 
Of course, this is not limited to console applications, 
you can also create an ASP.NET core application with Docker support, for example. However, 
this article has shown you how to integrate Docker into the development of.NET Core. 
In addition to deployment, Docker is ideal for test and development environments because you can set up other databases 
or the like without complex installations and compatibility problems with software already installed. 
Containers can be installed in parallel without any problems and thus have access to different options.
