---
id: 619
title: ServiceStack Logging with Slack Integration
date: 2017-12-06T23:30:43+00:00
author: Marcel Jurtz
layout: post
guid: https://blog.mjurtz.com/?p=619
permalink: /2017/12/servicestack-logging-with-slack-integration/
categories:
  - CSharp
tags:
  - .NET
  - CSharp
  - Development
  - Programming
  - ServiceStack
---
Besides my series on how to get started with ServiceStack, I have already published a few articles that are related to the topic, but do not belong directly to the introductory series. In the [last such article](https://blog.mjurtz.com/2017/12/query-coinmarketcap-api-using-servicestack/), I used the example of [coinmarketcap.com](https://coinmarketcap.com/) to show how to access third-party APIs using the C# client.

Today, I&#8217;ll show you how to use ServiceStacks Logging API and link it to a slack channel. The prerequisites for this article are an existing Slack account and a simple ServiceStack service ([see my article on this topic](https://blog.mjurtz.com/2017/11/servicestack-building-simple-service/)).

## Creating the Service

To be able to integrate your service to Slack, you need to login to your Slack account and create a [new Incoming Webhook](https://my.slack.com/services/new/incoming-webhook/). A default channel now needs to be specified. However, this will be overwritten by us later, so at this point the selection does not play an important role. After creating the webhook, Slack will provide an url that will be added to our logger in the next step.

Slack logging requires the nuget package _ServiceStack.Logging.Slack._ Add this to your project in Visual Studio. As soon as the package is installed, the base of the project is done. Next, the service will be adapted. The following source code must be added to the _Configure()_-method of your AppHost class:

{% highlight c# %}LogManager.LogFactory = new SlackLogFactory("webhook-url", debugEnabled: true)
{
    DefaultChannel = "logs",
    ErrorChannel = "errorlogs",
    BotUsername = "Sample ServiceStack Logger"
};{% endhighlight %}

Replace the first parameter with the URL of the previously created webhook and adjust the logger to cover your personal preferences. I use the channel &#8220;logs&#8221;as default for all messages, and &#8220;errorlogs&#8221; for all error messages. Similarly, a separate channel can be created for each logging type (debug, info, warning, error, fatal).

## Generating Logs

And that&#8217;s it! The logger can now be accessed from any part of the program. While each type of logging has its own method, in slack there is no automatic differentiation between them apart from the different channel distribution.

<pre class="EnlighterJSRAW" data-enlighter-language="null">public static ILog Log = LogManager.GetLogger(typeof(MyService));
Log.Info("Hello World!");
{% endhighlight %}