---
title: 'C# - Mastering the Basics - Interfaces'
date: 2018-03-04T26:18:00+00:00
author: Marcel Jurtz
layout: post
permalink: /2018/03/c-mastering-the-basics-interfaces/
categories:
  - CSharp
tags:
  - .NET
  - CSharp
  - Development
  - Programming
---

Interfaces are one of the things I learnt very early when getting started with programming, but they are also something I did not use really often.

You probably know interfaces as "things that describe what other things can do" or as "contracts that specify functionality of a class". While that's correct, it does not cover why interfaces are really useful.

One of the key advantages of using interfaces over concrete classes is the interchangeability of source code. To give you a quick example, let's take a look at the definition of List<T>. This class implements eight interfaces, which are:

* IList<T>
* ICollection<T>
* IEnumerable<T>
* IEnumerable
* IList
* ICollection
* IReadOnlyList<T>
* IReadOnlyCollection<T>

Each of these interfaces provide different functionality to the *List<T>* class. If you want to iterate over a list of items, *IEnumerable<T>* covers that for you. So if you simply want to iterate over items in your application and don't need other functionalities like index based object access, you can simple use *IEnumerable<T>* instead of *List<T>*.

The decisive factor here is that *IEnumerable<T>* is not only implemented by *List<T>*, but by various other generic collections. So if you decide later on to use a *Stack<T>* instead of a *List<T>*, you only need to update your instantiation, the rest of your code will work without any changes.

This very simple example gives you a small insight into what's possible by programming against an abstraction instead to a concrete class. The following sections will continue this thought and show some good practices & common use cases of interfaces.

## The Repository Pattern

## Explicit Implementation

## Interface Inheritance

## Dynamic Loading & Dependency Injection