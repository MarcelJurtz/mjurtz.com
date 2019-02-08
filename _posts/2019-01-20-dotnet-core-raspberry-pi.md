---
title: '.NET Core on a Raspberry Pi'
date: 2019-01-20T06:00:00+00:00
author: Marcel Jurtz
layout: post
permalink: /2019/01/dotnet-core-raspberry-pi
categories:
  - linux
tags:
  - programming
  - software
  - development
  - microsoft
  - dotnet
  - raspberrypi
---

The Raspberry Pi is one of my favorite toys when it comes to hosting local applications or services like relational databases for testing purposes. At the moment I am trying to test an ASP.NET core application with this setup. However, getting .NET Core to run on the Raspberry Pi is not as easy as I thought, so I'd like to briefly outline the necessary steps.

On [GitHub](https://github.com/dotnet/core/tree/master/release-notes/download-archives) you will find the download links we need. In my example I am using version 2.1.1. By following the link to the required version, you'll see a table with different download links.

Important for the Raspberry Pi are now the downloads for ARM architecture, I need the SDK and the ASP.NET Core Runtime. So I copy the two links and download the archives with wget:

{% highlight bash %}
wget https://download.microsoft.com/download/D/0/4/D04C5489-278D-4C11-9BD3-6128472A7626/dotnet-sdk-2.1.301-linux-arm.tar.gz 
wget https://download.microsoft.com/download/9/3/E/93ED35C8-57B9-4D50-AE32-0330111B38E8/aspnetcore-runtime-2.1.1-linux-arm.tar.gz 
{% endhighlight %}

I then create a new directory for .NET Core and extract both of the archives to this new directory:

{% highlight bash %}
sudo mkdir /opt/dotnet 
sudo tar -xvf dotnet-sdk-2.1.301-linux-arm.tar.gz -C /opt/dotnet/ 
sudo tar -xvf aspnetcore-runtime-2.1.1-linux-arm.tar.gz -C /opt/dotnet/ 
{% endhighlight %}

All that's left to do now is to create a symbolic link to the folder and you're good to go:

{% highlight bash %}
sudo ln -s /opt/dotnet/dotnet /usr/local/bin 
{% endhighlight %}

And that's it! Now you have access to the familiar .NET commands like dotnet build or run, and your ASP.NET core application is ready to run. 
