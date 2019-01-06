---
title: 'C# Attributes'
date: 2018-12-30T06:00:00+00:00
author: Marcel Jurtz
layout: post
permalink: /2018/12/csharp-attributes
categories:
  - development
tags:
  - programming
  - software
  - development
  - csharp
  - microsoft
---

During the development with C# you have certainly already encountered attributes, whether consciously or unaware. Today I want to go a little deeper into what attributes are, what kind of them are already present in the .NET framework, and how you can define your own attributes.

Microsoft defines attributes as a powerful way to associate metadata or descriptive information with code. But what exactly does that mean? 

Classes have, among other characteristics, fields, methods, properties, but also a state and behavior. You can "decorate" code elements with attributes to further describe them. Doing so, however, has no effect on their state.

You can imagine this a bit like tags or post-its attached to code and providing additional information.

Attributes are applicable at different times. While writing code, for example, debugging can be controlled, or additional instructions can be added to the compiler. A widespread use of attributes in this category is, for example, marking code elements as obsolete to indicate to developers that something should no longer be used.

At design time it is conceivable to control positioning etc. via attributes.

Finally, it is possible to use attributes at runtime. This is done for example for the validation of data in ASP.NET applications. At runtime, attributes can be evaluated using Reflection. Code execution can then be controlled by the presence or absence of attributes or based on their property values. I don't want to go into the validation using attributes separately in the course of this article, but you can find a detailed overview of the available attributes [here](https://docs.microsoft.com/de-de/dotnet/api/system.componentmodel.dataannotations?view=netframework-4.7.2).

There are some other use cases, some of which you have probably already used yourself. In the course of the article you will surely notice a few of them.

Attributes are enclosed in square brackets: `[MyCustomAttribute]`. By convention, the name of all attributes ends with "Attributes", which can be omitted. So it comes out the same whether you write `[ObsoleteAttribute]` or `[Obsolete]`.

Attributes can be used at different levels of code elements. On one hand this is directly on assembly level, when defining classes or structures, as well as interfaces and enumerations. But also methods, fields, properties or structures, events, delegates and single parameters can be decorated with attributes. In addition, an attribute can also be defined to the return value of a method.

It should be noted that not every attribute can be used for every code element. The author of the attribute specifies what it is valid for and where the invalid use throws a compile-time error.

## Inheritance of Attributes

Attributes on classes are inherited, that is, if class A is decorated with an attribute, class B, which inherits from class A, also has this attribute. You no longer have to specify this explicitly. However, this behavior can be deactivated at attribute level if required.

## Using Attributes

As mentioned above, attributes are used by using square brackets. An attribute itself is a normal class which inherits from System.Attribute. If you add an attribute to a code element, you can specify constructor parameters of the underlying class using regular parentheses. Nothing new, so far. 

There are different types of constructor parameters: required and optional parameters, and additional properties. As with normal methods, optional parameters are just parameters with default values. You can specify parameters comma separated as usual, and you can also use named arguments. If you want to use properties that cannot be filled by a constructor, this is also possible within the brackets. The syntax, however, is as follows: first the name of the property, followed by an equal sign and the corresponding value. As with regular classes, you can also use constructor-overloads for attributes, which are a bit clearer than the representation with properties.

Let's first take a look at some of the predefined attributes in C#. First, I'd like to briefly look at attributes that will help you when developing. I'll start with the DebuggerDisplay attribute: This is used to change the text that is displayed during debugging when you move the mouse over an object. To use it, all you have to do is specify a string to be used as text. You can also use curly braces to include property values. 

Another helpful attribute in this area is the DebuggerBrowsable attribute. This is defined at property level and indicates how you can inspect the element during debugging. The attribute has a required DebuggerBrowsableState constructor parameter, an enumeration that can take one of the values Collapsed, Never, and RootHidden. Never hides the property completely from the inspector, Collapsed collapses the nested elements and RootHidden is used for collections. The latter ensures that the elements of the collection itself are displayed in the inspector, but the main element is hidden.

Another attribute for adjusting the display in the debugger window is the DebuggerTypeProxy attribute. This attribute specifies a proxy class that is used instead of the original class to display the details in the debugger. For example, you can define a helper class here that displays various details of the actual class in a simplified way to save you cumbersome navigation in the debugging window. This is often useful for nested properties or combinations of multiple properties. At this point it should be noted that you can still view the properties of the underlying class, you'll find them at the bottom at the "Raw View" item when you hover the mouse over the object you want to inspect.

A widespread attribute, which I mentioned in the introduction and which I'm sure you've already seen when using external libraries in your applications, is the Obsolete attribute. This is used to mark code as deprecated and to tell the developer not to use this element anymore. If you want to use the attribute itself, you will notice that you can configure it in two ways: First, only a warning is generated, but you can still use the code element. The second variant is that the use throws a compilation error.

You can use the attribute with default constructor, so no arguments are required. In this case only a warning will be displayed, as well as when you move the mouse over the corresponding code element. But you can use it to adjust the behavior. There are two constructor overloads for this: In both cases you can specify a message here, which will be displayed as a hint, optionally you can also use boolean-flag to specify whether a compilation error should be generated.

An example of the attribute might look like this: 


{% highlight csharp %}
public class Student
{ 
  public string FirstName { get; set; } 

  public string LastName { get; set; } 

  [Obsolete("Use Property 'FormattedName' instead")] 
  public string GetFormattedName() { return $"{FirstName} {LastName}"; }

  public string FormattedName { get { return $"{FirstName} {LastName}"; } } 
} 
{% endhighlight %}

Accessing the GetFormattedName-Method displays the following warning: "Student.GetFormattedName()" is obsolete: Use Property 'FormattedName' instead.

I already mentioned that you can use attributes at the assembly level as well. To do this, you have to prefix the keyword "assembly:".

If you are working on a .NET Framework project, you can check out the "AssemblyInfo" class, which you can find in the "properties" folder. This class already contains some metadata declarations on your project like title, description or copyright. Of course, you can also define custom attributes here as well, just think of the "assembly:" prefix. If you're using .NET Core, you won't find such a file, since the metadata is generated automatically. However, you can override the values by adding assembly level attributes by yourself, which you can do at any position in your project. 

An attribute that is great if you are working on a project for which you want to write tests, but don't want to change it (for now), is InternalsVisibleTo. Suppose you are writing a test project for the very other project and want to test a class or method but can't access it due to its protection level, you can use this attribute to allow access by specifying the project name of your test project. This attribute is defined at assembly level and therefore does not have to be assigned to each class / method individually.

A less common feature of attributes is their use in conjunction with method return values. The attribute is specified above the method signature, including the prefix "return:".

Finally, attributes can be used for serialization. Use the "NonSerialized" attribute for properties that are not to be serialized. 

As you can see, you can already do a lot with the predefined attributes. Nevertheless, it is sometimes necessary to define your own attributes. This is what I want to show you now.

## Creating and using your own attributes

To restrict the usage of the attribute, the class is assigned another attribute: AttributeUsage. The constructor of this attribute requires an argument of type [AttributeTargets](https://docs.microsoft.com/de-de/dotnet/api/system.attributetargets?view=netframework-4.7.2), which is an enumeration from which you can select one or more code elements. If you want to support multiple code elements, you can do that by utilizing the bitwise-or operator like this: `[AttributeUsage(AttributeTargets.Property | AttributeTargets.Field)]`.

You can add another property to the constructor if you want to allow multiple use on a single code element. This is done with the AllowMultiple flag, the default value is false.

The last option to customize at this point is to control inheritance. By default, attributes are inherited with the inheritance of a class. This can be deactivated by setting the property "Inherited" to false.

So, enough for theory. Let's illustrate this with an example:

I define a simple attribute that I want to use later to control the output on the console. I inherit my custom attribute from System.attribute and end the name of the class with "attributes" according to the convention. The attribute has two properties, one of which is required and the other has a default value. In the case of a null value for the required parameter, an ArgumentException is thrown.


{% highlight csharp %}
[AttributeUsage(AttributeTargets.Property)] 
public class DisplayAttribute : Attribute 
{ 
  public DisplayAttribute(string tag, ConsoleColor textColor = ConsoleColor.White) 
  { 
    Tag = tag ?? throw new ArgumentException(nameof(tag)); 
    TextColor = textColor; 
  } 

  public string Tag { get; } 

  public ConsoleColor TextColor { get; } 
} 
{% endhighlight %}
 
Then I want to evaluate the attribute at runtime. To do this, I use Reflection, where I first evaluate a property for the presence of the attribute, and then I get the value. After that, I want to output the contents of the tag property, followed by the value of the actual property decorated with the attribute. The entire output should be in the color that is stored for the attribute.

I continue to use my student class from earlier, and create a new writer class to control the output: 

{% highlight csharp %}
public class Writer
{ 
  private readonly Student _student; 
  private ConsoleColor _color; 

  public Writer(Student student) 
  { 
    _student = student; 
  } 

  public void Write() 
  { 
    PropertyInfo firstNameProp = typeof(Student).GetProperty(nameof(Student.FirstName)); 

    DisplayAttribute firstNameDisplayAttr = (DisplayAttribute)Attribute.GetCustomAttribute(firstNameProp, typeof(DisplayAttribute)); 

    if (firstNameDisplayAttr != null) 
    { 
      SaveDefaultForegroundColor(); 
      Console.ForegroundColor = firstNameDisplayAttr.TextColor; 
      Console.WriteLine($"{firstNameDisplayAttr.Tag} {_student.FirstName}"); 
      RestoreDefaultForegroundColor(); 
    } 
    else 
    { 
      Console.WriteLine(_student.FirstName); 
    } 
  } 

  private void SaveDefaultForegroundColor() 
  { 
    _color = Console.ForegroundColor; 
  } 

  private void RestoreDefaultForegroundColor() 
  { 
    Console.ForegroundColor = _color; 
  } 
} 
{% endhighlight %}

As you can see, if the attribute is present, I save the current text color of the console to be able to reset it after the output. Then the output takes place. If the attribute does not exist, only the value of the property is printed to the console.

The section of reflection shown here is only a minimal part of the available options. For example, you could search an assembly for classes that have a specific attribute, and then use it to control the functionality of your application. You can also set or remove attributes at runtime. But I'll cover that with a separate article about Reflection.

I hope I was able to give you a general overview of attributes in C#. If you need more information, I recommend checking out MSDNs [full documentation on the topic](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/attributes/). 