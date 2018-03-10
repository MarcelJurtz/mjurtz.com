---
id: 406
title: Creating RESTful Services with ServiceStack
date: 2017-09-21T18:00:26+00:00
author: Marcel Jurtz
layout: post
guid: http://blog.mjurtz.com/?p=406
permalink: /2017/09/creating-restful-services-servicestack/
categories:
  - CSharp
tags:
  - .NET
  - ASP.NET
  - CSharp
  - Development
  - Programming
  - REST
  - ServiceStack
---
This articles covers the basic usage of [ServiceStack](https://servicestack.net/), a .NET-framework for creating RESTful services. The framework has a really great [documentation](http://docs.servicestack.net/), where you can check out all the details.

## Configuration

To get started, you&#8217;ll want to create a new ASP.NET project, and load the ServiceStack package via NuGet. Then, you&#8217;ll have to edit your _Web.config_ to look like the following (mentioned versions may differ from your project):

<pre class="EnlighterJSRAW" data-enlighter-language="generic">&lt;?xml version="1.0" encoding="utf-8"?&gt;
&lt;!--
  Informationen zur Konfiguration Ihrer ASP.NET-Anwendung finden Sie unter
  https://go.microsoft.com/fwlink/?LinkId=169433
  --&gt;
&lt;configuration&gt;
  &lt;system.web&gt;
    &lt;compilation debug="true" targetFramework="4.6.1"/&gt;
    &lt;httpRuntime targetFramework="4.6.1"/&gt;
    &lt;httpHandlers&gt;
      &lt;add path="*" type="ServiceStack.HttpHandlerFactory, ServiceStack" verb="*"/&gt;
    &lt;/httpHandlers&gt;
  &lt;/system.web&gt;
  &lt;!-- Required for IIS7 --&gt;
  &lt;system.webServer&gt;
    &lt;modules runAllManagedModulesForAllRequests="true"/&gt;
    &lt;validation validateIntegratedModeConfiguration="false" /&gt;
    &lt;handlers&gt;
      &lt;add path="*" name="ServiceStack.Factory" type="ServiceStack.HttpHandlerFactory, ServiceStack" verb="*" preCondition="integratedMode" resourceType="Unspecified" allowPathInfo="true" /&gt;
    &lt;/handlers&gt;
  &lt;/system.webServer&gt;
  &lt;system.codedom&gt;
    &lt;compilers&gt;
      &lt;compiler language="c#;cs;csharp" extension=".cs"
        type="Microsoft.CodeDom.Providers.DotNetCompilerPlatform.CSharpCodeProvider, Microsoft.CodeDom.Providers.DotNetCompilerPlatform, Version=1.0.5.0, Culture=neutral, PublicKeyToken=31bf3513ad364e35"
        warningLevel="4" compilerOptions="/langversion:default /nowarn:1659;1699;1701"/&gt;
      &lt;compiler language="vb;vbs;visualbasic;vbscript" extension=".vb"
        type="Microsoft.CodeDom.Providers.DotNetCompilerPlatform.VBCodeProvider, Microsoft.CodeDom.Providers.DotNetCompilerPlatform, Version=1.0.5.0, Culture=neutral, PublicKeyToken=31bf3513ad364e35"
        warningLevel="4" compilerOptions="/langversion:default /nowarn:41008 /define:_MYTYPE=\&quot;Web\&quot; /optionInfer+"/&gt;
    &lt;/compilers&gt;
  &lt;/system.codedom&gt;
&lt;/configuration&gt;</pre>

The second step is the configuration of the file _Global.asax.cs_. I&#8217;ve created a new class _HelloAppHost_ which inherits from _AppHostBase_.

<pre class="EnlighterJSRAW" data-enlighter-language="csharp">public class HelloAppHost : AppHostBase
{
    public HelloAppHost() : base("Hello Web Services", typeof(HelloService).Assembly) { }
    public override void Configure(Container container)
    {
        SetConfig(new HostConfig
        {
            DefaultContentType = MimeTypes.Json
        });

        // Authentication
        Plugins.Add(new AuthFeature(() =&gt; new AuthUserSession(),
            new IAuthProvider[] {
                new CustomCredentialsAuthProvider()
            }
        ));

        Plugins.Add(new RegistrationFeature());

        container.Register&lt;ICacheClient&gt;(new MemoryCacheClient());
        var userRep = new InMemoryAuthRepository();
        container.Register&lt;IUserAuthRepository&gt;(userRep);
    }
}</pre>

I&#8217;ve added a authentication functionality, which will be covered later in this post. The other thing I set up, is to return JSON-formatted data as default. The last thing to do in this file is creating an instance of the above class:

<pre class="EnlighterJSRAW" data-enlighter-language="csharp">protected void Application_Start(object sender, EventArgs e)
{
    var appHost = new HelloAppHost();
    appHost.Init();
}</pre>

## Adding Routes

After configuring the service, routes can be added. The general structure consists of three elements: RequestDTO, ResponseDTO and a Service. The following example shows how to set up a route following this approach:

<pre class="EnlighterJSRAW" data-enlighter-language="csharp">[Route("/hello/{name*}")]
public class Hello : IReturn&lt;HelloResponse&gt;
{
    public string Name { get; set; }
}

public class HelloResponse
{
    public string Result { get; set; }
}

public class HelloService : IService
{
    public object Get(Hello request)
    {
        var name = request.Name ?? "World";
        return new HelloResponse { Result = $"Hello, {name}!" };
    }
}</pre>

This service responds to requests on _/hello/{name*}_. The curly brackets indicate a variable, the star is a wildcard to indicate, that this variable is optional. This way, the service will also respond if no name is submitted (it will use the default &#8216;_World&#8217;_ for the response).

While you can use the notation above to mark routes, you can also set these up in your configuration (_Global.asax.cs_). Just add the following contents after _container.register_&#8230;

<pre class="EnlighterJSRAW" data-enlighter-language="csharp">Routes
    .Add&lt;Hello&gt;("/hello")
    .Add&lt;Hello&gt;("/hello/{Name}");</pre>

## HTTP Verbs

As shown in the example above, HelloService only implements the method _Get()_. This can be supplemented or extended by any other HTTP verb or Any, which will then respond to all requests on this path. ServiceStack also has a nice [documentation](http://docs.servicestack.net/design-rest-services) about the usage of each verb.

## Fallback Routes

You can use fallbacks to cover routes, that are not handled explicitly. In my case, I return information about the unhandled path and a timestamp. Again, wildcards are used.

<pre class="EnlighterJSRAW" data-enlighter-language="csharp">[FallbackRoute("/{Path*}")]
public class Fallback : IReturn&lt;FallbackResponse&gt;
{
    public String Path { get; set; }
}

public class FallbackResponse
{
    public string Message { get; set; }
    public string Path { get; set; }
    public String Timestamp { get; set; }
}

public class FallbackService : IService
{
    public object Any(Fallback request)
    {
        return new FallbackResponse {
            Message = "No matching path specified",
            Path = request.Path ?? "/",
            Timestamp = DateTime.Now.ToString()
        };
    }
}</pre>

## Authentication

The last element I&#8217;d like to cover is the authentication of a user. ServiceStack offers the class _CredentialsAuthProvider_, from which custom providers can be derived. The following example covers two methods, TryAuthenticate, which tries to log in by checking username and password against custom logic, and OnAuthenticated, which will be used to assign session variables.

<pre class="EnlighterJSRAW" data-enlighter-language="csharp">public class CustomCredentialsAuthProvider : CredentialsAuthProvider
{
    public override bool TryAuthenticate(IServiceBase authService,
    string userName, string password)
    {
        // Only checks if username equals password
        // implement custom logic here
        return userName.Equals(password);
    }

    public override IHttpResult OnAuthenticated(IServiceBase authService,
        IAuthSession session, IAuthTokens tokens,
        Dictionary&lt;string, string&gt; authInfo)
    {
        session.customElement= "Hello World";
        return base.OnAuthenticated(authService, session, tokens, authInfo);
    }
}</pre>

After creating this functionality, you can try to log in using the _/auth_ route via POST and adding a username and password parameter. If you want any of your routes to require authentication, just add the _[Authenticate]_ attribute above its response (above the [Route&#8230;]). After doing so, requesting this route without earlier authentication will lead to an empty response.

To log out, simply send a request (GET or POST) to _/auth/logout_.

## Further Information

As I mentioned above, [ServiceStacks documentation](http://docs.servicestack.net/) is really well written and maintained. I don&#8217;t think you&#8217;ll need any information besides what you can find there.