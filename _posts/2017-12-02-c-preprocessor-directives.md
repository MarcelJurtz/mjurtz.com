---
id: 603
title: 'C# Preprocessor Directives'
date: 2017-12-02T15:00:26+00:00
author: Marcel Jurtz
layout: post
guid: https://blog.mjurtz.com/?p=603
permalink: /2017/12/c-preprocessor-directives/
categories:
  - CSharp
tags:
  - .NET
  - CSharp
  - Development
  - Efficiency
  - Programming
---
You can use preprocessor directives in csharp to provide straightforward instructions to the compiler. For example, these directives allow you to execute certain code elements only under predefined conditions. Another possible field of use is simply the structuring of your source code into blocks, which can be folded in and out of Visual Studio.

In this article I will discuss some of these directives. Preprocessor directives are always started with a #-symbol.

## #define & #undef

The first two directives I would like to mention are _#define_ and _#undef_. They are used to define symbols and remove them. Imagine such a symbol as a simple variable, the next elements will help to illustrate it as well. Note, that _#define_-elements need to be at the very top of a file, even above the using-statements!

## #if, #else, #elif  & #else

The next symbols are _#if_, _#else_, _#elif_ and _#endif_. As you have probably already guessed, they are used to execute certain source code only under predefined conditions. And this is exactly where the symbols I defined in the previous section play a role. In the following source code I have defined a symbol, which is then checked for existence. According to this definition, the output _&#8220;Running in demomode!&#8221;_ will be performed. You can also use the predefined symbol _&#8220;DEBUG&#8221;_, to check wether your application runs in debug or release mode.

<pre class="EnlighterJSRAW" data-enlighter-language="null">#define DEMOMODE

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
}</pre>

VisualStudio automatically adjusts the display when saving, so that you can see directly in the editor which part of the if-statement will be executed.

As just mentioned, it is possible to use predefined symbols. You can edit these settings if you go to project settings and check the build tab.

<img src="https://i1.wp.com/blog.mjurtz.com/wp-content/uploads/2017/12/predefined_directives.png?resize=736%2C366&#038;ssl=1" alt="Predefined Directive Symbol" class="aligncenter size-full wp-image-650" width="736" height="366" srcset="https://i1.wp.com/blog.mjurtz.com/wp-content/uploads/2017/12/predefined_directives.png?w=736&ssl=1 736w, https://i1.wp.com/blog.mjurtz.com/wp-content/uploads/2017/12/predefined_directives.png?resize=500%2C249&ssl=1 500w" sizes="(max-width: 736px) 100vw, 736px" data-recalc-dims="1" />

## #region & #endregion

The last directives I would like to mention are #region and #endregion. These are used to split source code into related blocks.

The code within such a block can be expanded and collapsed by the plus and minus symbols in Visual Studio, which are displayed next to the row number bar. An optional name following the #region-remark allows you to assign a name to each block.

<img src="https://i1.wp.com/blog.mjurtz.com/wp-content/uploads/2017/12/region_endregion.png?resize=750%2C320&#038;ssl=1" alt="Region & Endregion" class="aligncenter wp-image-604 size-full" width="750" height="320" srcset="https://i1.wp.com/blog.mjurtz.com/wp-content/uploads/2017/12/region_endregion.png?w=879&ssl=1 879w, https://i1.wp.com/blog.mjurtz.com/wp-content/uploads/2017/12/region_endregion.png?resize=500%2C213&ssl=1 500w" sizes="(max-width: 750px) 100vw, 750px" data-recalc-dims="1" />