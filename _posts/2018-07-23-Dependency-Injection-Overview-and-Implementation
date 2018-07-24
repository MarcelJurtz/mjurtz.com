---
title: 'Dependency Injection - Overview and Implementation'
date: 2018-07-23T06:00:00+00:00
author: Marcel Jurtz
layout: post
permalink: /2018/07/dependency-injection-overview-and-implementation
categories:
  - programming
tags:
  - development
  - software
  - pattern
  - IoC
  - DI
  - CSharp
---

Especially when you are at the beginning of your career as a software developer, you probably know that: 
You have already picked up the term "Dependency Injection", but can't do anything with it directly? 
Then today's post is for you!

I will introduce you to the principle, functionality and the types of dependency injection. 
Finally, I'll show you how to write a simple IoC container. And don't worry if you are unfamiliar with the different terms, 
I will explain everything, you don't need any previous knowledge. 
However, certain basic knowledge in software development is definitely an advantage today. 
My code examples are written in C#, but you will have no problems understanding them if you are used to another programming language.

## What is Dependency Injection?

Dependency Injection (DI) is a term from the topic area "Inversion of Control" (IoC). 
IoC is a fundamental concept, DI is its implementation. An IoC container is a framework for dependency injection.

DI is the last point of Robert Martin's SOLID principle. 
Martin defines that high-level modules should not depend on low-level modules, instead an abstraction level should be used. 
The easiest way to get a grasp on the concept is to look at an example: 
Let's think of a program which processes input from the keyboard and sends it to a printer. 
This can be implemented by combining a class "printer" and "keyboard". So far, so good. 
However, this approach becomes problematic if the code should be extended, for example to support other input or output devices.

Martin's solution is to use abstraction layers that could be implemented as follows: 
Instead of the communication between the concrete classes "printer" and "keyboard", 
the abstractions "reader" and "writer" are used. These can be base classes, from which the concrete classes then inherit. 
The program flow only requires access to the base class, the implementation details are irrelevant. 
Changes and extensions are now possible without any problems.

## And what is Inversion of Control?

IoC is the underlying concept behind DI. As the name suggests, there is a inversion of control flow in the program. 
Different use cases are imaginable, for example inversion of control over the flow of the application, 
but also inversion of control over instantiation and binding of dependencies.

The first of these cases can be thought of as a comparison between a typical console application and a GUI application. 
The console application determines the process flow and waits for user interaction, 
while the user determines the process flow for the GUI application.

The second case aims to create required elements before they are required. In concrete terms, 
this means that objects are not created in the class in which they are needed, but before. 
They are then bound using constructor parameters or property setters, for example.

Other techniques that follow this approach include the factory pattern or the service locator pattern.

## DI in Action

Enough theory. The best way to understand the concept is to use examples. 
There are three types of dependency injection that I want you to understand:

Constructor Injection is probably the most commonly used type. 
A class is created and passed to the dependent class using constructor parameters 
instead of being instantiated in the dependent class itself.

The following example illustrates this with the class User, 
which accepts a constructor parameter of type IContactOption. 
The instance of an implementation of the interface is not created in the class itself, 
but is specified externally. The interface only has a SendMessage method.

{% highlight csharp %}
public class User
{
    private IContactOption _primaryContact;

    public User(IContactOption contactOption)
    {
        _primaryContact = contactOption;
    }
}

// …

IContactOption contactOption = new EMailContact();
User marcel = new User(contactOption);
{% endhighlight %}

The second category is the Setter Injection. No constructor parameter is used, but a property, which is set from the outside.

{% highlight csharp %}
public class User
{
    public IContactOption PrimaryContact;
}

// …

IContactOption contactOption = new EMailContact();
User marcel = new User();
marcel.PrimaryContact = contactOption;
{% endhighlight %}

The last category is a little bit more complicated, you will find this form in the real world much rarer than the other two. 
Interface injection is based on the dependent class implementing an interface for setting a variable.

{% highlight csharp %}
public interface IDependOnContactOption
{
  void Inject(IContactOption contactOption);
}

public class User : IDependOnContactOption
{
  private IContactOption _primaryContact;

  public void Inject(IContactOption contactOption)
  {
    _primaryContact = contactOption;
  }
}

// …

IContactOption contactOption = new EMailContact();
User marcel = new User();
marcel.Inject(contactOption);
{% endhighlight %}

Well, that's actually it. The implementation of the concept is far less complicated than the theory. 
However, we still lack the concrete application in our project, which will be discussed next. 
But first, I would like to issue a warning:

DI has a lot of advantages, but you should be careful with its use. 
DI leaks implementation details of your classes and thus contradicts the principle of encapsulation. 
In addition, the corresponding objects are always created before they are even needed. 
Here you should keep an eye on the performance of your application and decide when you really need the approach. 
DI also partially obscures problems in the structure of your classes. 
Testing your applications will be much easier because you can easily pass mocks, 
but you may overlook indications that a class should be divided into two or more components.

## Structure of an IoC Container

An IoC container is a framework for implementing dependency injection. 
The basic feature here is the automatic resolution of dependencies via an overall class, the so-called resolver. 
Let's look at the following implementation:

{% highlight csharp %}
public class Resolver
{
    public IContactOption ResolveContactOption()
    {
        return new EMailContact();
    }
}
{% endhighlight %}

This very simple example returns an EMailContact object for the IContactOption request. 
In practice, of course, you will have several options here, but there is already a problem with this implementation: 
it cannot be extended. Each interface I want to support requires its own method. 
I would prefer an implementation that allows something like: IContactOption contactOption = resolver.Resolve<IContactOption>();

And that is exactly what we want to implement now. 
This can be achieved by creating a Dictionary<Type, Type> to link the interfaces with the appropriate values. 
The entries must be accessible from the outside. 
Then, when requesting an implementation by calling resolve with its interface as argument, 
I will receive an instance of the concrete class.

To be able to test this more easily, I have adapted my previous classes as follows:

{% highlight csharp %}
public interface IContactOption
{
    void SendMessage(String message);
}

public class EMailContact : IContactOption
{
    public void SendMessage(String message)
    {
        Console.WriteLine("An email has been sent!");
    }
}

public class User
{
    private IContactOption _primaryContact;
   
    public User(IContactOption contactOption)
    {
        _primaryContact = contactOption;
    }

    public void Contact(String message)
    {
        _primaryContact.SendMessage(message);
    }
}
{% endhighlight %}

The resolver is implemented as follows: I've created  a method for registering classes and interfaces. 
The resolve-part is done by implementing a method that allows instances to be retrieved using the predefined matching rules. 
The resolving process is as follows: The system first checks whether a suitable entry is stored in the dictionary. 
If this is the case, the system checks whether the stored type has constructor parameters that must be instantiated. 
In the case of a default constructor without parameters, the type is instantiated and this instance is returned. 
Otherwise, the constructor parameters are collected and also instantiated. Here, too, the new instance is returned. 
This way, I'm able to support nested objects.

{% highlight csharp %}
public class Resolver
{
    private Dictionary<Type, Type> _registrations;

    public Resolver()
    {
        _registrations = new Dictionary<Type, Type>();
    }

    public void Register<TKey, TValue>()
    {
        _registrations.Add(typeof(TKey), typeof(TValue));
    }

    public T Resolve<T>()
    {
        return (T)Resolve(typeof(T));
    }

    private object Resolve(Type typeToResolve)
    {
        Type resolvedType = null;

        try
        {
            resolvedType = _registrations[typeToResolve];
        }
        catch(KeyNotFoundException)
        {
            throw;
        }

        var constructor = resolvedType.GetConstructors().First();
        var constParams = constructor.GetParameters();

        if (constParams.Count() == 0)
            return Activator.CreateInstance(resolvedType);

        IList<object> parameters = new List<object>();
        foreach(var parameter in constParams)
        {
            parameters.Add(Resolve(parameter.ParameterType));
        }

        return constructor.Invoke(parameters.ToArray());
    }
}
{% endhighlight %}

To be able to use the container now, only the registration of the relevant types is necessary. This may look like this:

{% highlight csharp %}
resolver.Register<User, User>();
resolver.Register<IContactOption, EMailContact>();

User marcel = resolver.Resolve<User>();
marcel.Contact("Hello World!");
{% endhighlight %}

The parameter IContactOption for User is automatically instantiated and assigned as EMailContact by the mapping. The call in the last line accordingly outputs the text "An email has been sent".

## Résumé

In a real application you will hardly implement your own IoC container, but use existing solutions. 
Nevertheless, I found the own implementation very helpful in order to be able to better understand the actual concept.
If you want to look at the available solutions, Unity (not the gaming engine), 
Castle Windsor and Ninject are probably the most widely used products (for C#). 
However, the way this works is all similar (though not quite as primitive) to our own implementation.
