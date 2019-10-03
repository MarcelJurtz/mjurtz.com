---
id: 569
title: 'ServiceStack - Building a Simple Service'
date: 2017-11-25T19:00:46+00:00
author: Marcel Jurtz
layout: post
guid: https://blog.mjurtz.com/?p=569
permalink: /2017/11/servicestack-building-simple-service/
categories:
  - csharp
tags:
  - dotnet
  - csharp
  - development
  - programming
  - servicestack
---
This is the second post in my series on building web services with ServiceStack. In [the first part](https://blog.mjurtz.com/2017/11/what-is-servicestack/), I covered the benefits of using ServiceStack, this article continues with the setup of a basic service and its several components.

In this and the following parts I will create a simple example project to illustrate the use of servicestack. This project will be an expenses tracker. In this part, we will develop the functionality to add expenses and establish an overall balance. In the following part we will combine them with the C# client.

The source code for this and the following parts can be found in [this GitHub repository](https://github.com/MarcelJurtz/BlogPosts_ServiceStack), where I will create a separate folder for each article. Since this is the second part of the tutorial, you'll find the source code for todays article in the folder [Part\_2\_Basic_Service](https://github.com/MarcelJurtz/BlogPosts_ServiceStack/tree/master/Part_2_Basic_Service).

## ServiceStack Service Architecture

Before we can create our first service, we need a basic understanding of how these work in ServiceStack. Basically, each service consists of three elements: a request, a response and an actual service. The request describes the object which is sent to the service, which proceeds the request and returns a reponse. We'll add a simple service to our project in the next step.

## Setting up the Server

Let's get right into development, where we'll start with the implementation of the server. Start by creating an empty ASP.NET web application in Visual Studio. There are some things you'll want to do now: First of all, install ServiceStack via nuget package manager. After that, adjust your web.config file so that it matches mine:

```xml <?xml version="1.0" encoding="utf-8"?>
<!--
  Informationen zur Konfiguration Ihrer ASP.NET-Anwendung finden Sie unter
  https://go.microsoft.com/fwlink/?LinkId=169433
  -->
<configuration>
  <system.web>
    <httpHandlers>
      <add path="*" type="ServiceStack.HttpHandlerFactory, ServiceStack" verb="*"/>
    </httpHandlers>
    <compilation debug="true"/>
  </system.web>
  <!-- Required for IIS7 -->
  <system.webServer>
    <modules runAllManagedModulesForAllRequests="true"/>
    <validation validateIntegratedModeConfiguration="false"/>
    <handlers>
      <add path="*" name="ServiceStack.Factory" type="ServiceStack.HttpHandlerFactory, ServiceStack" verb="*" preCondition="integratedMode" resourceType="Unspecified" allowPathInfo="true"/>
    </handlers>
  </system.webServer>
</configuration>```

The next step is to add a global.asax file. Do so by right-clicking your project - add new item - global.asax. In this file, the service going to be instantiated. However, you'll  first need a service which can be instantiated. Let's do so by adding our service class: _ExpensesService.cs_. This service is created as described in the previous section:

```csharppublic class ExpensesService : Service
{
    public object Post(Expense request)
    {
        return new ExpenseResponse
        {
            Amount = request.Amount,
            Total = 500,
            Status = "OK"
        };
    }
}

public class Expense
{
    public double Amount { get; set; }
}

public class ExpenseResponse
{
    public double Amount { get; set; }
    public double Total { get; set; }
    public String Status { get; set; }
}```

This service basically takes an amount as input for its request, and returns a new response object with a _Total_ of 500 and the _status_ _"OK"_. Of course, there is no logic in here currently, this will be added later. For now, we'll just want do get the service running.

Another thing which we need to set up is the previously created global.asax file. Simply add the following code inside the AppHostBase-class (the overriden class should already be there, you don't need to set this up manually).

```csharppublic class ExpenseTrackerAppHost : AppHostBase
{
    public ExpenseTrackerAppHost() : base("Expense Tracker", typeof(ExpensesService).Assembly) { }
    public override void Configure(Funq.Container container)
    {
        // Configure Application
    }
}

protected void Application_Start(object sender, EventArgs e)
{
    new ExpenseTrackerAppHost().Init();
}```

## The Meta Page

By now, the service should be able to run. When starting the project, you should be able to see a window like the following:

![The ServiceStack Meta Page](/assets/2017/servicestack_simple_service_01.png)

This overview provides information on all available services in our application. By clicking on the _JSON_-entry of the Expense-Operation, information about the service is displayed.

![The ServiceStack Meta Page - Detail](/assets/2017/servicestack_simple_service_02.png)

As you can see, this gives plenty of information on the service, including its route (/json/reply/Expense), its HTTP-verb (POST) and its parameters. Also, it shows us how the response-object will look like.

## Testing with Postman

To try this out, I use a tool called [Postman](https://www.getpostman.com/), which you can download as extension for chrome, but I recommend the standalone version, since the plugin is outdated. Postman allows us to test our routes and objects with the several HTTP-verbs and optional parameters.

![Testing ServiceStack with Postman](/assets/2017/servicestack_simple_service_03.png)

As you can see in the image, I set the HTTP-verb to POST and added the previously noted route. I also added an "Amount"-parameter, which ServiceStack then maps to the POCOs pendant. The response we get displays pretty much what we've expected.

## Adjusting the Routes

One thing that bothers me here, is the fixed route which looks pretty ugly. We can adjust this by adding a [Route]-Attribute to our service.

```csharp
[Route("/Expense")]
public class Expense
{
    public double Amount { get; set; }
}```

You can also specify multiple routes for one object, by adding curly braces you can add variables. If you want a variable to be optional, simply add an asterisk after the variable name. In the following example, both routes could be combined that way.

```csharp
[Route("/Expense")]
[Route("/Expense/{Amount}")]
public class Expense
{
    public double Amount { get; set; }
}```

Finally, note that ServiceStack doesn't recognize the format any longer with these custom routes. To clarify, what format you'll want to receive, simply add its ending to the route, like this: /Expense.json.

So that's it for todays post, the next one adds the C#-Client to easily communicate between C#-applications!