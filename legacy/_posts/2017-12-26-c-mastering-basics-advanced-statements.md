---
id: 693
title: 'C# – Mastering the Basics – Advanced Statements'
date: 2017-12-26T13:01:09+00:00
author: Marcel Jurtz
layout: post
guid: https://blog.mjurtz.com/?p=693
permalink: /2017/12/c-mastering-basics-advanced-statements/
categories:
  - csharp
tags:
  - dotnet
  - csharp
  - development
  - programming
---
In todays episode, I&#8217;ll show you how to use three advanced statements in C#. I&#8217;ll go over the so-called [null-conditional operator](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/null-conditional-operators), the [null-coalescing operator](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/null-conditional-operator) and the [ternary operator](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/conditional-operator).

## Null-Conditional Operator

You can use the null-conditional operator to prevent _NullReferenceException_s. The basic principle of the operator is to return a value if it is not null, and return null if it is. The following code shows you how to use the operator, which is initiated by a question mark, followed by a dot.

{% highlight c# %}List<String> stringList = null;
int? val = stringList?.Count;{% endhighlight %}

You can see the operator preceding the call of the Count-property. Running the code without the operator would lead to a NullReferenceException, but this way, _null_ is assigned to the variable _val_ instead.

## Null-Coalescing Operator

The next operator is the null-coalescing operator. It is initiated by two question marks and it&#8217;s used to assign different values based on wether the first choice is null or not. To get a better understanding of this, let&#8217;s look at an example:

{% highlight c# %}int? val = null;
int result = val ?? 0;{% endhighlight %}

The value of the variable _val_ is assigned to _result_. However, if _val_ is equal to null, _result_ will receive the value 0.

## Ternary Operator

The last operator in todays article is the ternary operator. It basically provides a short hand version for if-else-statements that can be written in a single line.

Let&#8217;s look at the following example:

{% highlight c# %}int val1 = 1;
int val2 = 2;
int smaller;

if (val1 > val2)
{
    smaller = val2;
}
else
{
    smaller = val1;
}{% endhighlight %}

The if-statement takes up a lot of space while its logic is very simple. This can be replaced by the following code using the ternary operator:

{% highlight c# %}int val1 = 1;
int val2 = 2;
int smaller = val1 > val2 ? val2 : val1;{% endhighlight %}

By using the ternary operator, you can simplify the assignment of variable by typing an expression that returns either true or false after the assignment operator. Complete the statement by a question mark. After that, the two values separated by a colon represent the values that will be assigned to the variable. The first one will be used if the statement is true, otherwise the second one is assigned.