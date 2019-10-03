---
id: 682
title: 'C# – Mastering the Basics - Extension Methods'
date: 2017-12-22T12:00:58+00:00
author: Marcel Jurtz
layout: post
guid: https://blog.mjurtz.com/?p=682
permalink: /2017/12/c-mastering-basics-extension-methods/
categories:
  - csharp
tags:
  - dotnet
  - dotnet-core
  - csharp
  - development
  - programming
---
In this series, I go over the very basics of the C# programming language. In todays guide, I will focus on writing custom extension methods for existing, prebuilt classes.

You can use extension methods to add custom functionality to classes that are built by others. In my very case, I'd like to extend the functionality of the String class to set its first letter to upper case. I already implemented this functionality in my sideproject [CryptoFolio](https://github.com/MarcelJurtz/CryptoFolio), be sure to check out [my article on the topic](https://blog.mjurtz.com/2017/12/sideproject-cryptofolio/)!

As you can see in [Microsofts documentation](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/extension-methods), extension methods are defined as static methods, but are called by using instance method syntax. This means that we will create a static method which can later be called by any object of the type _String_._ _While the implementation of such a method is very similar to a regular method, there is one thing you'll have to remember when writing extension methods: the _this_ keyword before the parameter. Let's see an example of how this works:

```csharppublic static class StringExtensions {
    public static String ToFirstLetterUpperCase(this String s)
    {
        if (s == null)
            return null;

        if (s.Length > 1)
            return char.ToUpper(s[0]) + s.Substring(1);

        return s.ToUpper();
    }
}```

In the example, I've added a new class called _StringExtensions_. This is the place where I will store all of my custom exension methods for strings. As you can see, the method looks very normal, the only thing different is the previously mentioned _this_ keyword right before the parameters data type.

After creating the extension method, it can be used by simply calling _ToFirstLetterUpperCase()_ on any string:

```csharpString s = "hello world!";
Console.WriteLine(s.ToFirstLetterUpperCase());
// Output: Hello world!```