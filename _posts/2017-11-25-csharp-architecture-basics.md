---
id: 552
title: 'C# &#8211; Mastering the Basics &#8211; Application Architecture'
date: 2017-11-25T12:00:05+00:00
author: Marcel Jurtz
layout: post
guid: https://blog.mjurtz.com/?p=552
permalink: /2017/11/csharp-architecture-basics/
categories:
  - CSharp
tags:
  - .NET
  - Clean Code
  - CSharp
  - Development
---
In this series, I discuss the optimization of the basics of programming in C#. This article deals with the basic structure of an application and its different components.

## C# Application Architecture

The top level always represents the so-called solution. A solution can contain several different projects, whereby a project can be used by different solutions, which allows to reuse source code. Each of a solutions projects compiles into either an executable or a dll.

The following image shows the structure of an example solution and its components.

![C# Example Architecture](csharp_example_architecture.png)

The application is divided into three levels: user interface layer, business logic layer and data access layer. The latter is usually realized by an object relational mapper like the entity framework or ORMLite, which I presented in [my last post](https://blog.mjurtz.com/2017/11/what-is-servicestack/). The UI layer contains the user views. These can be several projects, for example in the shape of a legacy WinForms application and a newer WPF implementation. The user interactions detected there are passed on to the business logic layer, which contains the actual logic of the software and the related POCOs.

To clarify this approach, projects of the Business logic layer are usually defined as class library projects, projects of the ui layers for example as WinForm.

The same logic is often required in more than a single application. An email functionality or the sending of push notifications would be conceivable. Such elements can be outsourced and referenced by multiple applications. These elements are located in the commons block.

## Types of C# Classes

According to this division, the connected classes must also be divided into several groups. A class is generally a template for an object that can be imagined as a casting mold.

For this purpose I differentiate between the types UI class, Domain entity class and library class. The UI class is the class that is automatically generated when a form is created. It is used to pass user interaction to the business logic. In addition, such a class can be implemented in the form of a ViewModel if it is developed according to the design pattern MVVM.

The domain entity class is used to describe entities in the business logic. Such a model therefore describes, for example, a customer or vendor, usually in the shape of a simple POCO. This also includes any repository classes that manage the matching entity classes in memory.

Finally, library classes represent classes that can be dynamically loaded and provide additional functionalities. These could be third-party libraries or components from the previously mentioned commons area.