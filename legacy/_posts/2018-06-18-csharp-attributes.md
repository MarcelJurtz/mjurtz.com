---
title: 'C# - Getting Started with Attributes'
date: 2018-06-18T06:00:00+00:00
author: Marcel Jurtz
layout: post
permalink: /2018/06/csharp-attributes
categories:
  - csharp
tags:
  - csharp
  - dotnet
  - development
  - programming
---

In todays post I want to show you how to use attributes in C#. I have to apologize in advance, though, as I'm going to focus only to a limited extent on why they make sense and what you can use them for, that's a topic for one of the following articles. Nevertheless, I would like to give you a basic understanding so that you can incorporate this concept into your own applications. 

So what are attributes? Well, attributes themselves are just classes. They can't do much themselves, but they can still be very helpful, especially on a topic called Reflection, which I will be discussing in the next days. 

Attributes are usually utilised to assign metadata to classes, properties and the like. Attributes inherit from the class *Attributes*. According to the convention, their names are always appended with the extension *Attributes*. They can be placed on various elements, one speaks of *decorating* a class, for example. To restrict the use of a custom attribute, the attribute *AttributeUsage* can be used. 

That was a whole bunch of attributes, right? Let us test this with an example, so that it will be easier to understand. 

{% highlight csharp %}
[AttributeUsage(AttributeTargets.Class | AttributeTargets.Property)]  
class DemoAttribute : Attribute 
{ 
  public string Description { get; set; } 
} 
{% endhighlight %}

I created my own attribute, DemoAttribute. I have designed the name according to the convention. I want only properties and classes to be decorated with the attribute, which is why I piped these two options together using the AttributeUsage attribute.

The attribute has a property called Description. With this I want to describe my classes and properties later. Let us also look at an example.

{% highlight csharp %}
[Demo(Description = "Test for class A")] 
class ClassA 
{ 
  [Demo(Description = "Test for Property ClassA.Name")]
  public string Name { get; set; } 
}

[Demo(Description = "This is class B")] 
class ClassB 
{
} 

class ClassC 
{ 
} 
{% endhighlight %}

I created three classes to test my attribute: ClassA, ClassB and ClassC. I know, very incredibly creative. I decorated classes A and B with my attribute, class C I deliberately ignored. In addition, class A has a property with the attribute. 

Now I want to show you the real purpose of attributes, but for this I have to be a little ahead of schedule. If you are interested in the relevant topics, just search for the keyword *Reflection* (oh, and LINQ, if you are new to this). 

{% highlight csharp %}
var classes = from t in Assembly.GetExecutingAssembly().GetTypes() 
  where t.GetCustomAttributes<DemoAttribute>().Count() > 0 
  select t; 

foreach(var c in classes) 
{ 
  Console.WriteLine($"Fount class: {c.Name}"); 
} 
{% endhighlight %}

In this snippet I first obtain all classes in the current assembly that are decorated with the attribute *DemoAttribute*. Then I iterate through the result set and simply output the names of the respective classes. 

You can add the following to search the found classes for properties with the same attribute. 

{% highlight csharp %}
var props = from p in c.GetProperties() 
  where p.IsDefined(typeof(DemoAttribute), false) 
  select p; 

foreach(var p in props) 
{ 
  Console.WriteLine($"Fount property: {p.Name}");
} 
{% endhighlight %}

Of course, the concept can be extended as far as you like. For example, you can design method calls so that all elements within an assembly or across assemblies are called in this way, thus modularizing your application without creating references between projects. But more about this later. With this short introduction I have only shown the basic idea of attributes. As already mentioned, the use of attributes is a separate topic. Stay tuned! 