---
title: 'Building your first API in .NET Core'
date: 2018-10-21T06:00:00+00:00
author: Marcel Jurtz
layout: post
permalink: /2018/10/your-first-api-net-core
categories:
  - programming
tags:
  - software
  - net
  - netcore
  - development
---

Today I want to show you how to develop your first API with .NET Core.  

.NET Core is an alternative to the classic .NET Framework. In contrast to the .NET Framework, .NET Core offers a huge advantage: It is platform-independent and therefore compatible with MacOS, Linux and Windows.

The purpose of this article is to build a small REST-based API, which will respond to HTTP GET-requests. Of course you can extend the project according to your own ideas and experiment around.

# Setup and Installation

First of all you need to install the .NET Core SDK. You can download it directly from [Microsoft](https://www.microsoft.com/net/download), where you will find two different links. On the one hand you will find the Runtime Environment, as well as the SDK, which also contains the RE, but additionally tools for development. Download and install the SDK for the platform of your choice.

In addition, it is a good idea to use an IDE for development. You can use Visual Studio for this, or alternatively you can use Visual Studio Code or JetBrains Rider.

Since I'm currently working on Linux, I'll use Visual Studio Code.

# Create a project  

When working with .NET Core, the command line is used a lot at the moment. One such point is the creation of a project. Basically you can use any command line interface here, be it the terminal integrated in VS Code or the Windows Power Shell.  

The first command you need to know is dotnet new.  This command initializes a new project, appending the type of the project. For each dotnet command, you can display operating information by appending *help*. In the case of *dotnet new help* you will be shown among other things the possible project types.  

We want to create a new ASP .NET Core MVC project. But I don't want to have a preconfiguration, so I initialize my new project with *dotnet new web*.

# Adaptation of the project & implementation of the API

First the Startup.cs class needs to be reworked. I've made some minor changes, which you can find in the following snippet:  

{% highlight csharp %} 
using System; 
using System.Collections.Generic; 
using System.Linq; 
using System.Threading.Tasks; 
using Microsoft.AspNetCore.Builder; 
using Microsoft.AspNetCore.Hosting; 
using Microsoft.AspNetCore.Http; 
using Microsoft.AspNetCore.Mvc; 
using Microsoft.Extensions.DependencyInjection; 

namespace ApiSample 
{ 
  public class Startup 
  { 
    // This method gets called by the runtime. Use this method to add services to the container. 
    // For more information on how to configure your application, visit https://go.microsoft.com/fwlink/?LinkID=398940 
    public void ConfigureServices(IServiceCollection services) 
    {
      services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
    } 

    // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
    public void Configure(IApplicationBuilder app, IHostingEnvironment env) 
    {
      if (env.IsDevelopment()) 
      { 
        app.UseDeveloperExceptionPage(); 
      } 
      else 
      { 
        // Enforce HTTPS for Requests 
        app.UseHsts(); 
      } 

      app.UseHttpsRedirection(); 
      app.UseMvc(); 
    } 
  } 
}
{% endhighlight %} 

Once everything is configured, the API can be implemented. I want to build a really simple implementation that returns the status of the service. An HTTP GET request should be answered with an info message telling the caller that the API is running.  

First of all we need a model class, which I built as follows:  

{% highlight csharp %} 
namespace ApiSample.Models 
{ 
  public class ApplicationState 
  { 
    public string Information { get; set; } 
  } 
} 
{% endhighlight %} 

The heart of the API is the controller that handles the requests. So I add a new controller to my project that responds to GET requests via the */api* route. The implementation looks like this:  

{% highlight csharp %}
namespace ApiSample.Controllers 
{ 
  [Route("api/")] 
  [ApiController] 
  public class SampleController : ControllerBase 
  { 
    public ActionResult<ApplicationState> GetApplicationState() 
    { 
      return new ApplicationState() { Information = "API is running" }; 
    } 
  } 
} 
{% endhighlight %} 

If you have already worked with older versions of ASP .NET, you may have been using IActionResult as the return type. Now you can directly use the generic class ActionResult, where the type in braces is the type of the model which is stored in the response. On the one hand, this helps you with the readability of your methods, since the result is directly visible. This in turn is very helpful if you use things like automatic documentation based on method headers.

Basically the structure as you can see is kept very simple. And that's it for now. Start your project with the command *dotnet run*, you will get the address and port where the application is running. With a browser of your choice or a test tool like Postman you can now test the API. By calling the address with the attachment */api* you should finally get the expected status message.

# Further information

This article really only shows the information you need to get started with the topic. If I could arouse your interest, you can read on directly at [Microsoft](https://docs.microsoft.com/en-us/aspnet/core/?view=aspnetcore-2.1). But of course there are also several other articles about the topic, let your creativity run free and just try a little bit around. 