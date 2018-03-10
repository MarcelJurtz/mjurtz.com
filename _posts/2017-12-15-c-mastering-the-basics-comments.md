---
id: 660
title: 'Comments &#038; how to use them'
date: 2017-12-15T22:58:08+00:00
author: Marcel Jurtz
layout: post
guid: https://blog.mjurtz.com/?p=660
permalink: /2017/12/c-mastering-the-basics-comments/
categories:
  - Tips
tags:
  - Clean Code
  - Development
  - Efficiency
  - Programming
---
Comments are used in a lot of programming languages. While this concept is useful for a lot of purposes, beginners often start bad habits when commenting their code. In this article I will explain when comments are useful and on the other hand illustrate problems where commenting can be less meaningful or even disturbing.

## Bad Comments &#8230;

At first sight (especially at the beginning of your programming career), you might think of comments as the perfect way to describe elements of your code that you don&#8217;t fully understand. This approach is not entirely wrong, but it does have a few problems that I will now discuss.

### Comments as excuse for unreadable code

A problem of overly intensive commenting is the justification of illegible code. According to the motto &#8220;the code is not readable, but the commentary explains everything I need&#8221; the whole code quickly becomes unreadable and it takes a lot more time to understand its functionality. A simple example of this is the following code, which in the form of static methods offers the possibility to convert temperatures from Fahrenheit to celsius and vice versa.

<pre class="EnlighterJSRAW" data-enlighter-language="csharp">class Converter
{
    /* Celsius to Fahrenheit
     * input: temperature in celsius
     */
    public static double cToF(double x)
    {
        return x * 9 / 5 + 32;
    }

    /* Celsius to Fahrenheit
     * input: temperature in celsius
     */
    public static double fToC(double x)
    {
        return (x - 32) * 5 / 9;
    }
}</pre>

A much better approach for this very simple example could be implemented like the following, which will work without any comments and will still be understood much better than the previous variant.

<pre class="EnlighterJSRAW" data-enlighter-language="csharp">class TempConverter
{
    public static double FahrenheitToDecimal(double fahrenheit)
    {
        double celsius = (fahrenheit - 32) * 5 / 9;
        return celsius;
    }

    public static double CelsiusToFahrenheit(double celsius)
    {
        double fahrenheit = celsius * 9 / 5 + 32;
        return fahrenheit;
    }
}</pre>

### Readability of the Actual Code

Another important point you should consider is that comments can even decrease the readability of your code.  Of course, your source code will be hard to read if you have to read through ten lines of comments before each line of code. This of course a bit exaggerated, but the point becomes clear.

This problem has to be considered especially in combination with the first one. By using comments to describe even the smallest of activities, there is a risk that source code will be poorly structured. Also, you&#8217;ll most likely spent less time thinking about naming things, which can reduce readability drastically.

### Code Changes and Obsolete Comments

The last problem I would like to address is the change of code. The more comments written, the higher the probability of changing code and not adapting a descriptive comment. I&#8217;m sure that you are aware of this and probably stumbled across that topic more than one time.

However, this is usually not a very big problem, since the source code itself explains how a method works, for example. This is more difficult if you depend on the content of the comments. Accordingly, this problem also combines with the two previous points.

While source code typically changes quite quickly, comments often remain unchanged. If the comment now describes a functionality but is obsolete, this can easily lead to confusion.

## &#8230; and how to avoid them

Enough of the problems that come with comments. Comments are by no means to be considered as ballast and are definitely useful. With this article, however, I want to create a certain basis to deal with this functionality in a more deliberate way.

While comments are helpful to keep track of your thoughts, this tool should be used with thoughtfulness. Excessive use can easily lead to the opposite of the expected result. However, it is also important to note that the code alone is definitely not documentation enough. Of course, this does not mean that the entire project documentation should be in the form of comments, but the possibility of commenting on code should certainly not be ignored.

All in all, I recommend that you think about the meaning of it before adding a comment. Are there other ways to communicate the content of the comment? Do I only use it to describe the functionality of a method? In this case, you should prefer to start with checking whether the naming of the components is clear enough. On the other hand, an argument for the use of comments could be the documentation of unexpected result values or the like, i. e. anything that is not quickly apparent from the source code itself.