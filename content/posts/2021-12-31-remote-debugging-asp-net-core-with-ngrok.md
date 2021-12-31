---
title: 'Remote Debugging ASP.NET Core with Ngrok'
date: 2021-12-31T06:00:00+00:00
author: Marcel Jurtz
layout: post
permalink: /2021/12/remote-debugging-asp-net-core-with-ngrok
categories:
  - development
tags:
  - webdevelopment
  - aspnetcore
  - dotnet
  - debugging
  - tools
---

Sometimes it can be helpful to test your apps not only on a local machine, but also using other devices within or outside, your network.
In my current example, I use an ASP.NET Core web application that contains an API that I can in turn access via an Android app.
I debug the Android app on a physical device that is on the same network as the ASP.NET Core application.

One option now would be to access the API via local web service, publishing the ASP.NET Core application on my dev machine.
I have not yet found a sensible alternative to set up remote access already in debug mode (message me if I'm wrong).
During my search for solutions to access the service without having to republish after every change, I finally stumbled across [ngrok](https://ngrok.com/), a tunneling solution for local applications.

The procedure is very simple: start ngrok via a terminal command, which sets up a tunnel for a certain port of your machine.
Ngrok then provides you with an ngrok.io-domain, which can be reached externally and remains open until the application is closed again.

The simple variant looks like this:

```bash
./ngrok http 8080
```

Replace 8080 with the respective port of your application. So far, so good. However, there's one problem remaining. When a request comes in from a web browser, IIS Express uses the host header to determine which website to load. If the website is accessed through the public URL, the host header sent by the the host header sent by the web browser does not match the one expected by IIS Express. Luckily, ngrok provides an additional parameter for this:

```bash
./ngrok http 8080 -host-header="localhost:8080"
```

Keep in mind that the ngrok.io-address changes every time you start ngrok. If you access it with other applications, remember to update it each time.