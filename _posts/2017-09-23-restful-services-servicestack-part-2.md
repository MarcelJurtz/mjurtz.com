---
id: 418
title: 'RESTful Services with ServiceStack &#8211; Part 2'
date: 2017-09-23T18:00:14+00:00
author: Marcel Jurtz
layout: post
guid: http://blog.mjurtz.com/?p=418
permalink: /2017/09/restful-services-servicestack-part-2/
categories:
  - csharp
tags:
  - dotnet
  - asp-dotnet
  - csharp
  - development
  - programming
  - rest
  - servicestack
---
In my [last post](http://blog.mjurtz.com/2017/09/creating-restful-services-servicestack/), I&#8217;ve set up a basic RESTful service using ServiceStack. This article deals with the implementation of a corresponding functionality on the client side. The application will query the path provided by the server and apply the pre-defined authentication method, requiring the user to log in with username and password before the application delivers the desired content.

Both projects, client and server are located on my [GitHub-profile](https://github.com/MarcelJurtz/ServiceStackExamples) and can be viewed and downloaded there.

The structure of the client is very simple. There are two textboxes for entering username and password, as well as one for entering a name for the _hello/{name*}_ path. The button sends the request and the richtextbox is filled with the response.

![ServiceStack Client Demo Application](/assets/2017/servicestack-client-demo.png)

Access to the classes created for the server-side service is required. For this reason, these were also added to the project. This includes _Hello_ and _HelloResponse_.

{% highlight c# %}
[Route("/hello/{name*}")]
public class Hello : IReturn<HelloResponse>
{
    public string Name { get; set; }
}

public class HelloResponse
{
    public string Result { get; set; }
}
{% endhighlight %}

The same applies to the custom AuthenticationProvider.

{% highlight c# %}
public class CustomAuthenticationProvider : CredentialsAuthProvider
{
    public override bool TryAuthenticate(IServiceBase authService, string userName, string password)
    {
        // Only checks if username equals password
        // implement custom logic here
        return userName.Equals(password);
    }
    public override IHttpResult OnAuthenticated(IServiceBase authService, IAuthSession session, IAuthTokens tokens, Dictionary<string, string> authInfo)
    {
        //session.customElement = "Hello World";
        return base.OnAuthenticated(authService, session, tokens, authInfo);
    }
}
{% endhighlight %}

An additional class has been created to control access to the service. This encapsulates the requests, which can be easily accessed from outside.

{% highlight c# %}
using System;
using ServiceStack;

namespace RESTfulDemoClient
{
    class RESTfulServiceClient : IDisposable
    {
        private const String url = "http://localhost:64424";
        private JsonServiceClient serviceclient;

        private JsonServiceClient getServiceClient(String usern, String passw)
        {
            if(serviceclient == null)
            {
                serviceclient = new JsonServiceClient(url);

                var authResponse = serviceclient.Post(new Authenticate
                {
                    provider = CustomAuthenticationProvider.Name, //= credentials
                    UserName = usern,
                    Password = passw,
                    RememberMe = true,
                });

                serviceclient.AlwaysSendBasicAuthHeader = true;
            }
            return serviceclient;
        }

        public HelloResponse GetHelloResponse(Hello request, String username, String password)
        {
            var client = this.getServiceClient(username, password);
            var response = client.Post(request);
            return response;
        }

        public void Dispose()
        {
            serviceclient = null;
        }
    }
}
{% endhighlight %}

The class already contains all the logic required for access control, and the method _GetHelloResponse_ can be used to retrieve a response from the _Hello_-path, which is implemented like this:

{% highlight c# %}
using(var client = new RESTfulServiceClient())
{
    var request = new Hello() { Name = txtRequest.Text };
    try
    {
        var response = client.GetHelloResponse(request, txtUsername.Text, txtPassword.Text);
        txtResponse.Text = response.Result;
    }
    catch (ServiceStack.WebServiceException ex)
    {
        txtResponse.Text = ex.StatusCode + " - " + ex.ErrorMessage;
    }
}
{% endhighlight %}