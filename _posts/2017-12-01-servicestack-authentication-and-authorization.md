---
id: 589
title: 'ServiceStack &#8211; Authentication and Authorization'
date: 2017-12-01T15:00:40+00:00
author: Marcel Jurtz
layout: post
guid: https://blog.mjurtz.com/?p=589
permalink: /2017/12/servicestack-authentication-and-authorization/
categories:
  - CSharp
tags:
  - .NET
  - CSharp
  - Development
  - Programming
  - REST
  - ServiceStack
---
This is the fourth part on my series on how to get started using ServiceStack. Be sure to check out the earlier articles, if you haven&#8217;t read them already:

  * [Part 1 &#8211; What is ServiceStack and why should I use it?](https://blog.mjurtz.com/2017/11/what-is-servicestack/)
  * [Part 2 &#8211; Building a Simple Service](https://blog.mjurtz.com/2017/11/servicestack-building-simple-service/)
  * [Part 3 &#8211; Using the C#-Client](https://blog.mjurtz.com/2017/11/servicestack-using-the-c-client/)

Today, I&#8217;ll be adding basic authentication and authorization to the project we&#8217;ve created over the last parts. As always, you can get the source code from GitHub.

## Authentication vs. Authorization

To be able to use authentication and authorization, we&#8217;ll first need to know the difference between both of these.

Authentication basically means to provide a guarantee that you are who you pretend to be. Usually, such a functionality is implemented via username / password combinations.

Authorization on the other hand is used to define, what a user is allowed to do. For example, you might be authorized to moderate comments on a forum.

## Authentication in ServiceStack

To authenticate a user, ServiceStack offers several options, check out the [full documentation](http://docs.servicestack.net/authentication-and-authorization). Among others, the following methods are available.

  * Credentials (username & password combination)
  * Basic Auth (HTTP Basic Auth)
  * Custom Credentials (Custom login implementation, uses username & password combination)

As I mentioned above, this list is by no means complete, I just picked some methods that I&#8217;d like to explain in detail in this article. Another option available is authentication via OAuth. This means that you can allow your users to authenticate themselves using their twitter, github, facebook [or other](http://docs.servicestack.net/authentication-and-authorization#oauth-providers) accounts.

### HTTP Basic Auth

I&#8217;d like to go over the detials of implementing authentication functionality by using the HTTP Basic Auth implementation of ServiceStack. The goal of the modifications of our sourcecode is to allow the transfer service only after successful authentication by the user.

Do you remember the _Configure-_method of our _ExpenseTrackerAppHost-_class, which has remained completely empty so far?
  
We&#8217;ll start with this one. The mentioned method servers the instantiation of plugins for ServiceStack. These include various modules that are built into ServiceStack, but which are only available after manual activation. An example of this is the API documentation with Swagger. We will use two such plugins: The _AuthFeature_-Plugin to enable authentication, and the _InMemoryAuthRepository_ to be able to easily test our code without having to maintain actual users.

The server configuration needs to be adjusted as shown in the snippet below. The first entry you see are the two mentioned elements. Next I created a user, which we can use to test the service.

<pre class="EnlighterJSRAW" data-enlighter-language="csharp">public override void Configure(Funq.Container container)
{
    Plugins.Add(new AuthFeature(() =&gt;
        new AuthUserSession(), new IAuthProvider[] {
        new BasicAuthProvider() }));

    var userRepository = new InMemoryAuthRepository();
    container.Register&lt;IUserAuthRepository&gt;(userRepository);

    string hash;
    string salt;

    new SaltedHash().GetHashAndSaltString("password", out hash, out salt);
    
    userRepository.CreateUserAuth(new UserAuth
    {
        Id = 1,
        DisplayName = "Marcel Jurtz",
        Email = "jurtz@example.com",
        UserName = "MJurtz",
        FirstName = "Marcel",
        LastName = "Jurtz",
        PasswordHash = hash,
        Salt = salt
    }, "password");
}</pre>

After the server has been configured, you&#8217;ll need to specify which routes require authentication. This is done simply by adding the [Authenticate]-attribute above the specific request-dtos class, like this:

<pre class="EnlighterJSRAW" data-enlighter-language="csharp">[Authenticate]
[Route("/Expense")]
[Route("/Expense/{Amount}")]
public class Expense : IReturn&lt;ExpenseResponse&gt;
{
    public double Amount { get; set; }
}</pre>

The service now won&#8217;t return any valid data as long as the user is not logged in. This can be tested by using postman, which will return HTTP statuscode 401 (unauthorized) for any request. If, however, the &#8220;_Authorization_&#8221; option (oh, the irony) is activated in the header data, the login works.

### Custom Credentials Authorization

Another approach I&#8217;d like to cover is the implementation of a custom authentication mechanism. By extending CredentialsAuthProvider and overriding the methods _TryAuthenticate_ and _OnAuthenticated_, you&#8217;re able to implement your very own login functionality, for example to match existing business logic. The following example simply checks for hardcoded credentials to demonstrate the basic functionality.

<pre class="EnlighterJSRAW" data-enlighter-language="csharp">public class CustomCredentialsProvider : CredentialsAuthProvider
{
    public override bool TryAuthenticate(IServiceBase authService, string userName, string password)
    {
        return userName == "MJurtz" && password == "password";
    }

    public override IHttpResult OnAuthenticated(IServiceBase authService, IAuthSession session, IAuthTokens tokens, Dictionary&lt;string, string&gt; authInfo)
    {
        return base.OnAuthenticated(authService, session, tokens, authInfo);
    }
}</pre>

_TryAuthenticate()_ is used to test for valid login credentials, _OnAuthenticated()_ will be executed after successful login. To test this, we use postman again. If you use your own mechanisms for authentication, it is necessary to send a request to _/Authenticate_ first. Here, username and password have to be provided in the body-field. Once this request has been successfully completed, ServiceStack&#8217;s automatic session functionality provides access to our service as usual.

### Login with the C#-Client

Of course, you&#8217;ll want to be able to access the service with the ServiceStack C#-Client, which I introduced in [part 3 of this series](https://blog.mjurtz.com/2017/11/servicestack-using-the-c-client/).

ServiceStack offers an easy way to access username and password via the C# client. In the case of the _BasicAuthProvider_ this works as follows:

<pre class="EnlighterJSRAW" data-enlighter-language="csharp">JsonServiceClient client = new JsonServiceClient("http://localhost:61401") { UserName = "MJurtz", Password = "password" };</pre>

In the case of a custom implementation, access via client is minimally more complicated:

<pre class="EnlighterJSRAW" data-enlighter-language="csharp">var authResponse = jsonServiceClient.Post(new Authenticate
{
    provider = "credentials",
    UserName = "MJurtz",
    Password = "password",
    RememberMe = true
});</pre>

## Authorization in ServiceStack

As explained at the beginning of the article, we differentiate between authentication and authorization. After the first part has been completed, we will now cover the second part.

With regard to authorization in ServiceStack, there are two big points we want to consider: roles and permissions. With the help of permissions we can assign certain rights to individual users. Users can also be grouped into roles, which in turn have certain rights.

Once again, ServiceStack offers a convenient way to manage rights. However, both options work very similarly, so I will not go into the individual elements in detail.

In addition to the Authenticate request from earlier, services can be provided with additional attributes that control the access rights. For filtering to a certain role, this attribute is _[RequiredRole (&#8220;User&#8221;)]_, the counterpart for the permission is _[RequiredPermission (&#8220;DoSomething&#8221;)]_.

We will continue the example with the BasicAuthProvider created at the beginning and assign rights to the user created there. This is similar in both cases.

In addition to the Authenticate request from earlier, services can be provided with additional attributes that control the access rights. For filtering to a certain role, this attribute is[RequiredRole (&#8220;User&#8221;)], the counterpart for the permission is[RequiredPermission (&#8220;DoSomething&#8221;).

We will continue the example with the BasicAuthProvider created at the beginning and assign rights to the user created there. Again, this is similar in both cases. To provide roles and permissions to a user, simply add the specific element either to the Roles- or Permissions-List.

<pre class="EnlighterJSRAW" data-enlighter-language="csharp">Roles = new List&lt;string&gt; { "User" }
Permissions = new List&lt;string&gt; { "DoSomething"}</pre>

As an alternative to my hardcoded roles and permissions, you can of course transfer them to constants or, for more practical purposes, to a database.

###