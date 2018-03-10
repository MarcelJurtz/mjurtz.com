---
id: 569
title: 'ServiceStack &#8211; Building a Simple Service'
date: 2017-11-25T19:00:46+00:00
author: Marcel Jurtz
layout: post
guid: https://blog.mjurtz.com/?p=569
permalink: /2017/11/servicestack-building-simple-service/
categories:
  - CSharp
tags:
  - .NET
  - CSharp
  - Development
  - Programming
  - ServiceStack
---
This is the second post in my series on building web services with ServiceStack. In [the first part](https://blog.mjurtz.com/2017/11/what-is-servicestack/), I covered the benefits of using ServiceStack, this article continues with the setup of a basic service and its several components.

In this and the following parts I will create a simple example project to illustrate the use of servicestack. This project will be an expenses tracker. In this part, we will develop the functionality to add expenses and establish an overall balance. In the following part we will combine them with the C# client.

The source code for this and the following parts can be found in [this GitHub repository](https://github.com/MarcelJurtz/BlogPosts_ServiceStack), where I will create a separate folder for each article. Since this is the second part of the tutorial, you&#8217;ll find the source code for todays article in the folder [Part\_2\_Basic_Service](https://github.com/MarcelJurtz/BlogPosts_ServiceStack/tree/master/Part_2_Basic_Service).

## ServiceStack Service Architecture

Before we can create our first service, we need a basic understanding of how these work in ServiceStack. Basically, each service consists of three elements: a request, a response and an actual service. The request describes the object which is sent to the service, which proceeds the request and returns a reponse. We&#8217;ll add a simple service to our project in the next step.

## Setting up the Server

Let&#8217;s get right into development, where we&#8217;ll start with the implementation of the server. Start by creating an empty ASP.NET web application in Visual Studio. There are some things you&#8217;ll want to do now: First of all, install ServiceStack via nuget package manager. After that, adjust your web.config file so that it matches mine:

<pre class="EnlighterJSRAW" data-enlighter-language="xml">&lt;?xml version="1.0" encoding="utf-8"?&gt;
&lt;!--
  Informationen zur Konfiguration Ihrer ASP.NET-Anwendung finden Sie unter
  https://go.microsoft.com/fwlink/?LinkId=169433
  --&gt;
&lt;configuration&gt;
  &lt;system.web&gt;
    &lt;httpHandlers&gt;
      &lt;add path="*" type="ServiceStack.HttpHandlerFactory, ServiceStack" verb="*"/&gt;
    &lt;/httpHandlers&gt;
    &lt;compilation debug="true"/&gt;
  &lt;/system.web&gt;
  &lt;!-- Required for IIS7 --&gt;
  &lt;system.webServer&gt;
    &lt;modules runAllManagedModulesForAllRequests="true"/&gt;
    &lt;validation validateIntegratedModeConfiguration="false"/&gt;
    &lt;handlers&gt;
      &lt;add path="*" name="ServiceStack.Factory" type="ServiceStack.HttpHandlerFactory, ServiceStack" verb="*" preCondition="integratedMode" resourceType="Unspecified" allowPathInfo="true"/&gt;
    &lt;/handlers&gt;
  &lt;/system.webServer&gt;
&lt;/configuration&gt;</pre>

The next step is to add a global.asax file. Do so by right-clicking your project &#8211; add new item &#8211; global.asax. In this file, the service going to be instantiated. However, you&#8217;ll  first need a service which can be instantiated. Let&#8217;s do so by adding our service class: _ExpensesService.cs_. This service is created as described in the previous section:

<pre class="EnlighterJSRAW" data-enlighter-language="csharp">public class ExpensesService : Service
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
}</pre>

This service basically takes an amount as input for its request, and returns a new response object with a _Total_ of 500 and the _status_ _&#8220;OK&#8221;_. Of course, there is no logic in here currently, this will be added later. For now, we&#8217;ll just want do get the service running.

Another thing which we need to set up is the previously created global.asax file. Simply add the following code inside the AppHostBase-class (the overriden class should already be there, you don&#8217;t need to set this up manually).

<pre class="EnlighterJSRAW" data-enlighter-language="csharp">public class ExpenseTrackerAppHost : AppHostBase
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
}</pre>

## The Meta Page

By now, the service should be able to run. When starting the project, you should be able to see a window like the following:

<img src="https://i0.wp.com/blog.mjurtz.com/wp-content/uploads/2017/11/service_gui.png?resize=500%2C377&#038;ssl=1" alt="ServiceStack Service GUI" class="aligncenter size-medium wp-image-571" width="500" height="377" srcset="https://i0.wp.com/blog.mjurtz.com/wp-content/uploads/2017/11/service_gui.png?resize=500%2C377&ssl=1 500w, https://i0.wp.com/blog.mjurtz.com/wp-content/uploads/2017/11/service_gui.png?w=770&ssl=1 770w" sizes="(max-width: 500px) 100vw, 500px" data-recalc-dims="1" />

This overview provides information on all available services in our application. By clicking on the _JSON_-entry of the Expense-Operation, information about the service is displayed.

<img src="https://i0.wp.com/blog.mjurtz.com/wp-content/uploads/2017/11/service_gui_2.png?resize=500%2C520&#038;ssl=1" alt="ServiceStack Service Detail" class="aligncenter size-medium wp-image-572" width="500" height="520" srcset="https://i0.wp.com/blog.mjurtz.com/wp-content/uploads/2017/11/service_gui_2.png?resize=500%2C520&ssl=1 500w, https://i0.wp.com/blog.mjurtz.com/wp-content/uploads/2017/11/service_gui_2.png?w=772&ssl=1 772w" sizes="(max-width: 500px) 100vw, 500px" data-recalc-dims="1" />

As you can see, this gives plenty of information on the service, including its route (/json/reply/Expense), its HTTP-verb (POST) and its parameters. Also, it shows us how the response-object will look like.

## Testing with Postman

To try this out, I use a tool called [Postman](https://www.getpostman.com/), which you can download as extension for chrome, but I recommend the standalone version, since the plugin is outdated. Postman allows us to test our routes and objects with the several HTTP-verbs and optional parameters.

<img src="https://i0.wp.com/blog.mjurtz.com/wp-content/uploads/2017/11/service_postman.png?resize=481%2C399&#038;ssl=1" alt="Testing ServiceStack with Postman" class="aligncenter size-full wp-image-573" width="481" height="399" srcset="https://i0.wp.com/blog.mjurtz.com/wp-content/uploads/2017/11/service_postman.png?w=481&ssl=1 481w, https://i0.wp.com/blog.mjurtz.com/wp-content/uploads/2017/11/service_postman.png?resize=360%2C300&ssl=1 360w" sizes="(max-width: 481px) 100vw, 481px" data-recalc-dims="1" />

As you can see in the image, I set the HTTP-verb to POST and added the previously noted route. I also added an &#8220;Amount&#8221;-parameter, which ServiceStack then maps to the POCOs pendant. The response we get displays pretty much what we&#8217;ve expected.

## Adjusting the Routes

One thing that bothers me here, is the fixed route which looks pretty ugly. We can adjust this by adding a [Route]-Attribute to our service.

<pre class="EnlighterJSRAW" data-enlighter-language="csharp">[Route("/Expense")]
public class Expense
{
    public double Amount { get; set; }
}</pre>

You can also specify multiple routes for one object, by adding curly braces you can add variables. If you want a variable to be optional, simply add an asterisk after the variable name. In the following example, both routes could be combined that way.

<pre class="EnlighterJSRAW" data-enlighter-language="null">[Route("/Expense")]
[Route("/Expense/{Amount}")]
public class Expense
{
    public double Amount { get; set; }
}</pre>

Finally, note that ServiceStack doesn&#8217;t recognize the format any longer with these custom routes. To clarify, what format you&#8217;ll want to receive, simply add its ending to the route, like this: /Expense.json.

So that&#8217;s it for todays post, the next one adds the C#-Client to easily communicate between C#-applications!