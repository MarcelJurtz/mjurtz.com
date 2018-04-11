---
id: 737
title: 'C# – Mastering the Basics – Operator Overloading'
date: 2018-01-21T11:00:20+00:00
author: Marcel Jurtz
layout: post
guid: https://blog.mjurtz.com/?p=737
permalink: /2018/01/c-mastering-the-basics-operator-overloading/
categories:
  - csharp
tags:
  - dotnet
  - csharp
  - development
  - programming
---
Most of the built-in operators that are available in C# can be redefined. In todays article, I&#8217;ll be showing you how you can redefine operators to match your custom classes. Additionally, I&#8217;ll cover which operators actually can be overwritten.

## <span>Redefining Operators<br /> </span>

To create custom functionality for operators in combination with your own classes, you&#8217;ll simply need to create methods matching a specific pattern. These methods require the usage of the keyword _operator_, followed by the actual operator you&#8217;ll want to overwrite.

I&#8217;ve created a simple class to demonstrate the concept of operator overloading. This class represents a rectangle, which only has the two properties _Length_ and _Width_.

{% highlight c# %}public class Rectangle
{
    public double Length { get; set; }
    public double Width { get; set; }
}{% endhighlight %}

I&#8217;d like to start by overloading the + operator. When this is applied to two rectangles, I want to receive a new rectangle where both width and length from both input objects are added. You achieve this functionality by adding the following method to the class _Rectangle_.

{% highlight c# %}public static Rectangle operator + (Rectangle a, Rectangle b)
{
    return new Rectangle { Length = a.Length + b.Length, Width = a.Width + b.Width };
}{% endhighlight %}

Quite easy, isn&#8217;t it? But of course, you&#8217;re not restricted to these simple operators, which is why I&#8217;ll cover all of the available operators in the next section.

Another example I&#8217;d like to demonstrate is the comparison of two objects. In the following listing, you can see the comparison of two Rectangle objects with the operators _==_ and _!=_.

{% highlight c# %}public static bool operator == (Rectangle a, Rectangle b)
{
    return (a.Length == b.Length && a.Width == b.Width);
}

public static bool operator != (Rectangle a, Rectangle b)
{
    return (a.Length != b.Length || a.Width != b.Width);
}{% endhighlight %}

## (Non-)Overloadable Operators

Altough a lot of operators can be overloaded, there are some exceptions. To give you a better understanding and overview of allowed operators, I&#8217;ve listed them in the table below.

<table style="border-collapse: collapse; border-spacing: 0;">
  <tr>
    <th style="font-weight: bold; padding: 10px 5px; overflow: hidden; word-break: normal; text-align: center; border: 1px solid #000000;">
      Operator
    </th>
    
    <th style="font-weight: bold; padding: 10px 5px; overflow: hidden; word-break: normal; text-align: center; border: 1px solid #000000;">
      Description
    </th>
    
    <th style="font-weight: bold; padding: 10px 5px; overflow: hidden; word-break: normal; text-align: center; border: 1px solid #000000;">
      Overloadable
    </th>
  </tr>
  
  <tr>
    <td style="padding: 10px 5px; overflow: hidden; word-break: normal; border: 1px solid #000000;">
      +, -, *, /
    </td>
    
    <td style="padding: 10px 5px; overflow: hidden; word-break: normal; border: 1px solid #000000;">
      Basic arithmetic operations
    </td>
    
    <td style="padding: 10px 5px; overflow: hidden; word-break: normal; color: #009901; text-align: center; border: 1px solid #000000;">
      ✓
    </td>
  </tr>
  
  <tr>
    <td style="padding: 10px 5px; overflow: hidden; word-break: normal; border: 1px solid #000000;">
      ++, &#8212;
    </td>
    
    <td style="padding: 10px 5px; overflow: hidden; word-break: normal; border: 1px solid #000000;">
      Increment / Decrement
    </td>
    
    <td style="padding: 10px 5px; overflow: hidden; word-break: normal; color: #009901; text-align: center; border: 1px solid #000000;">
      ✓
    </td>
  </tr>
  
  <tr>
    <td style="padding: 10px 5px; overflow: hidden; word-break: normal; border: 1px solid #000000;">
      ~
    </td>
    
    <td style="padding: 10px 5px; overflow: hidden; word-break: normal; border: 1px solid #000000;">
      Bitwise complement
    </td>
    
    <td style="padding: 10px 5px; overflow: hidden; word-break: normal; color: #009901; text-align: center; border: 1px solid #000000;">
      ✓
    </td>
  </tr>
  
  <tr>
    <td style="padding: 10px 5px; overflow: hidden; word-break: normal; border: 1px solid #000000;">
      ==, !=, <, >, <=, >=
    </td>
    
    <td style="padding: 10px 5px; overflow: hidden; word-break: normal; border: 1px solid #000000;">
      Comparison
    </td>
    
    <td style="padding: 10px 5px; overflow: hidden; word-break: normal; color: #009901; text-align: center; border: 1px solid #000000;">
      ✓
    </td>
  </tr>
  
  <tr>
    <td style="padding: 10px 5px; overflow: hidden; word-break: normal; border: 1px solid #000000;">
      &&, ||
    </td>
    
    <td style="padding: 10px 5px; overflow: hidden; word-break: normal; border: 1px solid #000000;">
      Conditional logical operators
    </td>
    
    <td style="padding: 10px 5px; overflow: hidden; word-break: normal; color: #cb0000; text-align: center; border: 1px solid #000000;">
      ✗
    </td>
  </tr>
  
  <tr>
    <td style="padding: 10px 5px; overflow: hidden; word-break: normal; border: 1px solid #000000;">
      +=, -=, *=, /=, %=
    </td>
    
    <td style="padding: 10px 5px; overflow: hidden; word-break: normal; border: 1px solid #000000;">
      Assignment Operators
    </td>
    
    <td style="padding: 10px 5px; overflow: hidden; word-break: normal; color: #cb0000; text-align: center; border: 1px solid #000000;">
      ✗
    </td>
  </tr>
</table>