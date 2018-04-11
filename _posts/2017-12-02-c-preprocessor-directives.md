---
id: 603
title: 'C# Preprocessor Directives'
date: 2017-12-02T15:00:26+00:00
author: Marcel Jurtz
layout: post
guid: https://blog.mjurtz.com/?p=603
permalink: /2017/12/c-preprocessor-directives/
categories:
  - csharp
tags:
  - dotnet
  - csharp
  - development
  - efficiency
  - programming
---
You can use preprocessor directives in csharp to provide straightforward instructions to the compiler. For example, these directives allow you to execute certain code elements only under predefined conditions. Another possible field of use is simply the structuring of your source code into blocks, which can be folded in and out of Visual Studio.

In this article I will discuss some of these directives. Preprocessor directives are always started with a #-symbol.

## #define & #undef

The first two directives I would like to mention are _#define_ and _#undef_. They are used to define symbols and remove them. Imagine such a symbol as a simple variable, the next elements will help to illustrate it as well. Note, that _#define_-elements need to be at the very top of a file, even above the using-statements!

## #if, #else, #elif  & #else

The next symbols are _#if_, _#else_, _#elif_ and _#endif_. As you have probably already guessed, they are used to execute certain source code only under predefined conditions. And this is exactly where the symbols I defined in the previous section play a role. In the following source code I have defined a symbol, which is then checked for existence. According to this definition, the output _&#8220;Running in demomode!&#8221;_ will be performed. You can also use the predefined symbol _&#8220;DEBUG&#8221;_, to check wether your application runs in debug or release mode.

{% highlight c# %}
#define DEMOMODE

using System;

namespace CompilerDirectivesDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            #if DEMOMODE

            Console.WriteLine("Running in demomode!");

            #else
            
            Console.WriteLine("Not running in demomode!");
            
            #endif

            Console.ReadLine();
        }
    }
}{% endhighlight %}

VisualStudio automatically adjusts the display when saving, so that you can see directly in the editor which part of the if-statement will be executed.

As just mentioned, it is possible to use predefined symbols. You can edit these settings if you go to project settings and check the build tab.

![Preprocessor Directives](/assets/2017/preprocessor_directives.png)

## #region & #endregion

The last directives I would like to mention are #region and #endregion. These are used to split source code into related blocks.

The code within such a block can be expanded and collapsed by the plus and minus symbols in Visual Studio, which are displayed next to the row number bar. An optional name following the #region-remark allows you to assign a name to each block.

![Preprocessor Directives - Regions](/assets/2017/preprocessor_directives_regions.png)