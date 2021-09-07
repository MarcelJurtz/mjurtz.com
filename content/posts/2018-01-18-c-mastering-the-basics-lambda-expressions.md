---
id: 724
title: 'C# – Mastering the Basics – Lambda Expressions'
date: 2018-01-18T17:52:31+00:00
author: Marcel Jurtz
layout: post
guid: https://blog.mjurtz.com/?p=724
permalink: /2018/01/c-mastering-the-basics-lambda-expressions/
categories:
  - csharp
tags:
  - dotnet
  - csharp
  - development
  - programming
---
Are you one of the people who are easily intimidated by complicated-looking things? I felt a little bit like that when I saw lambda expressions for the first time. But let me tell you this: they really only look scary on the very first look. This article covers the basics of using lambda expressions and should be seen as quick introduction rather than extensive documentation.

## What are Lambda Expressions?

Lambda expressions somehow belong to the anonymous methods. However, they are not called _expression_ instead of _method_ for nothing. They are therefore only similar, not identical. Under a certain circumstance, i. e. the assignment of a delegate, they can be used identically.

By the way, anonymous methods are methods that cannot be called simply because of a missing name.

Lambda expressions are often used when working with LINQ, but they are also applied in asynchronous programming.

## How can they be used?

A lambda expression consists of three elements: the parameters, the Lambda operator and the statements. Any number of input parameters can be used. The number of instructions is also theoretically up to you, but in practice a limitation of three statements is widely accepted. Otherwise the readability of your source code will suffer. Moreover, Lambda statements are also meant for short inserts anyway.

Enough talk, here's an example:

```csharpFunc<string, string, string> concatShort = (x, y) => x + " " + y;
Console.WriteLine(concatShort("Hello", "World"));```

This basic example shows you how to create a method using lambda expressions. The input consists of two variables (x and y, name them whatever you want) and the method will concatenate two strings and return them. The third _string_ entry in the Func<> signature is reserved for the return type.

And that's the most basic usage. There are some important rules, which I will wrap up in the following:

  * If you have only one input parameter, you can omit the brackets
  * The typing of the parameters is optional
  * If only one statement is used, the brackets AND the _return_ keyword can be omitted

As you might be used to from LINQ, lambda expressions are only evaluated on execution. This means that variable values are interesting at the time of execution, not earlier. At this point it is equally important to say that it is possible to access variables which are available outside the lambda expression but within the method defining it. Let me demonstrate this:

```csharpint variable = 0;
Action print = () => Console.WriteLine(outerValue);
variable = 42;
print(); // Output will be 42```

## Further Usage and Examples

What exactly is the point of lambda expressions? Simply a shorter version of anonymous methods are no justification of a new construct in C#. And that's the point. LINQ has given lambda expressions their right to exist in C#. Anonymous methods alone were no longer sufficient.

If you simply assign a variable of the type System.Linq.Expression<TDelegate> to a lambda expression instead of a delegate, you get the ability to analyze and generate it at runtime.
  
The result of this is called _"Expression Tree"_, which has the important restriction to consist of only one expression. But that goes beyond the scope of this article.

<span>Let's get back to the point:</span> lambda expressions are used in many other areas. Let's showcase some of them:

The following example shows a method that accepts two integer arrays as parameter and returns their difference. So, the result is an integer array containing all items from a, that are not included in b. As you can see, there is only one input parameter and one statement, which allows me to omit the brackets.

```csharppublic static int[] ArrayDiff(int[] a, int[] b)
{
  return a.Where(x=>!b.Contains(x)).ToArray();
}```

My second example checks wether a string is a pangram and stores this information as a boolean variable. A pangram is a sentence that contains each letter from the alphabet.

```csharpbool isPangram = str.ToLower().Where(c => Char.IsLetter(c)).GroupBy(c => c).Count() == 26;```