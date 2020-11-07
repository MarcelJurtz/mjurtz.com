---
title: 'FluentValidation in ASP.NET Core'
date: 2020-10-05T06:00:00+00:00
author: Marcel Jurtz
layout: post
permalink: /2020/10/fluentvalidation-in-asp-net-core
categories:
  - csharp
---

Have you been implementing the validation of your ViewModels in ASP.NET (Core) using DataAttributes so far? If so, chances are you've always been dissatisfied because your classes became messy and full of unnecessarily replicated code. Yes? Then today's post is for you.

## FluentValidation

FluentValidation is a way to define strongly typed validation for classes in the form of a Fluent API. The term *fluent* in this case refers to a code structure where a function always return the modified object. This way, you can chain together multiple method calls and produce very readable source code, especially in configurations where it is often used. You may already be familiar with this structure from areas like the ASP.NET setup or LINQ.

FluentValidation is a popular library for defining validation rules in .NET and is available open source at [GitHub](https://github.com/FluentValidation). Oh, and before I jump into the topic: the [projects documentation](https://docs.fluentvalidation.net/en/latest/index.html) is very good and should be more than sufficient for most of your questions.

## Installation & Setup

To use FluentValidation, you first need to install corresponding NuGet package *FluentValidation.AspNetCore*. After that, the only thing left to do is the configuration in your applications startup class. Just add a call to *AddFluentValidation* in the method *ConfigureServices* and you're all set! To work correctly with the DI infrastructure, the validators must be defined here afterwards tough:

```csharp
public void ConfigureServices(IServiceCollection services) {
    services.AddMvc()
        // ...
        .AddFluentValidation();
    
    services.AddTransient<IValidator<ClubMember>, ClubMemberValidator>();
    // ...
}
```

As you may guess, that would of course get very messy very quickly, so let's check for another way to achieve that. Fortunately, the authors also thought of that and provided another solution: the automatic registration of all validators inside a given assembly. The only two requirements to these is that they are a) marked as public and b) inherit the generic *AbstractValidator*-class.

To do that, you just need to customize the call to *AddFluentValidation* as follows:

```csharp
.AddFluentValidation(fv => fv.RegisterValidatorsFromAssemblyContaining<Startup>());
```

This will now register all validators, that are contained inside the same assembly as *Startup*. Knowing that, we can just jump straight into defining our first validator. In my case, that's the one for the *ClubMember*-class, which could look like this:

```csharp
public class ClubMember {
    public int Id { get; set; }
    public int MemberId { get; set; }
    public int ClubId { get; set; }
    public string Role { get; set; }
    public int Number { get; set; }
}

public class ClubMemberValidator : AbstractValidator<ClubMember> {
    public ClubMemberValidator() {
	    (x => x.Role).Length(3, 25);
        RuleFor(x => x.Number).InclusiveBetween(1, 100);
        // ...
    }
}
```

There are multiple predefined validationrules available in FluentValidation. However, you can of course create your own if you need to support a specific usecase. The validation itself is now set up, and you can check it by calling *ModelState.IsValid* inside your controller actions. 

## Pitfalls

There are a few things to be considered when using the library. For example, the regular asp.net validation using DataAttributes is still active and will be applied after the fluentvalidation. It may be helpful to clean up old code to avoid unexpected side effects.

Another point to keep in mind is that complex objects are not automatically checked further down in their hierarchical structure. If you want to your validator to validate child objects by default, you have to specify this either on the object itself or in the configuration. To achieve this, you can use the property *ImplicitlyValidateChildProperties*.

FluentValidation itself is meant to be used for server-side validation. However, it can also be used to provide metadata for validation on the client side. You can find more information on this topic in the project documentation linked above.

## TL;DR

In my opinion, FluentValidation offers a very nice way to define strongly typed validation rules in ASP.NET (Core), without having to mess up your classes. However, there are many more details I haven't covered yet. As I mentioned earlier, the documentation of the project is a very good place to get answers to potential questions and general information, but you also might find posts on various details here in the future.