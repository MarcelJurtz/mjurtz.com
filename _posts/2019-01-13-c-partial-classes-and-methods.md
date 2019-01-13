---
title: 'Partial Classes and Methods in C#'
date: 2019-01-13T06:00:00+00:00
author: Marcel Jurtz
layout: post
permalink: /2018/12/c-partial-classes-and-methods
categories:
  - development
tags:
  - programming
  - software
  - development
  - microsoft
  - dotnet
---

The *partial* keyword in C# allows you to spread a class definition across multiple files. You've probably seen this before when you created a WinForms application and viewed the MyForm.Designer.cs file, which contains the properties you define in the Designer. So the Visual Designer stores the entire definition of your user interface in this separate file. The class of your form itself is always marked *partial*. Files of partial methods must not be in different assemblies and will be merged during compilation. The IL code therefore looks exactly the same as if the class were a single file.

This example also shows you the main purpose of partial classes: Code generation, without accidentally changing existing code of the programmer or complicated evaluations, which code now belongs to the designer.

Basically, this feature is a good way to split class components and thus make the file content clearer. However, it is often discouraged from overusing partial classes, as the goal of clarity can easily result in  the opposite. In short - When used prudently, partial classes offer a great feature to split code files thematically and provide more clarity. It would be conceivable, for example, to implement interfaces in your own files, so that the corresponding members can be found more quickly.

But now to the probably less known feature in this matter: partial methods. Partial methods can only be defined in partial classes and generally have the return type *void*.  It is also not possible to bypass this condition using out parameters. Also not allowed are access modifiers and keywords like *virtual*, *sealed*, *override*, *new* or *abstract*.

On the one hand, partial methods are always defined with the method header only (quasi as in abstract classes), the implementation then takes place elsewhere. It is possible to define several method headers with different overloads. The implementation itself always takes place in one piece, so at this point nothing is distributed to several places in the code.

If you stumble across partial methods in a project, you will usually find the definition of the method and its implementation in different files, as this is the only way to get the idea behind the concept. This code element is also usually found in generated code. If a partial method is called and an implementation of it is found, it is called and executed normally. However, if no implementation is found, the method (or more precisely the method header) and the calls to it are not compiled at all. Partial methods thus indicate optional additions to the source code.