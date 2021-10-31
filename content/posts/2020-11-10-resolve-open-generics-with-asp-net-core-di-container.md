---
title: 'Resolve Open Generics with ASP.NET Cores DI-Container'
date: 2020-11-10T06:00:00+00:00
author: Marcel Jurtz
layout: post
permalink: /2020/11/resolve-open-generics-with-asp-net-core-di-container
categories:
  - development
tags:
  - dependency injection
  - dotnet core
---

ASP .NET Core already has quite a decent di-container built in. However, there are some quirks you might need to be aware of. In one of my applications, I've been using a factory pattern to register my views by using open generics. To be more clear about that: I have an Interface *IView&lt;TView&gt;*, and I want to register all of this interfaces implementations.

As I found out, Microsofts DI-Container currently doesn't support that by default (text me if I'm wrong!), but there's a solution for this problem.
[Scrutor](https://github.com/khellang/Scrutor) is a package that adds more complex di-features - specifically *scanning*. This means that, using this library, you now have the ability to scan through your assemblies and perform registrations based on your findings.

Now, for my case, this was quite simple: All I had to do was to add the following snippet to my startup-class:

```csharp
services.Scan(scan =>
  scan.FromAssembliesOf(typeof(IView<>))
    .AddClasses(classes =>
    classes.AssignableTo(typeof(IView<>)).Where(x => !x.IsGenericType))
        .AsImplementedInterfaces()
        .WithTransientLifetime());
```

And that's it! Now, all the implementations of the interface find their way into my di container and I can easily retrieve new instances by utilizing the following factory class:

```csharp
public class ViewFactory : IViewFactory
{
    private readonly IServiceProvider _services;
    public ViewFactory(IServiceProvider services)
    {
        _services = services;
    }

    public Task<TView> CreateViewAsync<TView>()
    {
        var builder = _services.GetService<IView<TView>>();

        if (builder == null) 
        {
          throw new Exception($"View {typeof(TView)} not registered");
        }
          
        return builder.BuildAsync();
    }
}
```

If needed, you could easily extend this by a second type parameter to specify e.g. arguments to pass to the view. Those would then be passed to the *CreateViewAsync*-Method, which in turn would forward them to the builders *BuildAsync*-Method.