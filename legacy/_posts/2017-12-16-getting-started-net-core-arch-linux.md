---
id: 669
title: Getting Started with .NET Core on Arch Linux
date: 2017-12-16T16:40:56+00:00
author: Marcel Jurtz
layout: post
guid: https://blog.mjurtz.com/?p=669
permalink: /2017/12/getting-started-net-core-arch-linux/
categories:
  - csharp
tags:
  - dotnet
  - dotnet-core
  - development
  - linux
  - programming
---
Hello Internet! Today, we will try to develop and run .NET Core Apps on a Macbook running Arch Linux! Because what could go wrong!

(Honestly, this went way better than I thought.)

## Installation

Before we can start writing .NET Core applications on Linux, we need to install some packages. The [Arch Wiki](https://wiki.archlinux.org/index.php/.NET_Core) states that to run .NET Core applications, the _dotnet-runtime_ package needs to be installed. Additionally, we will need _dotnet-sdk-2.0_ from the [AUR](https://aur.archlinux.org/packages/dotnet-sdk-2.0/) to be able to develop custom applications.

Besides these packages, I recommend installing Visual Studio Code as text editor. I currently use this as my main text editor and it offers a plugin for C# development. You can download it from the [AUR](https://aur.archlinux.org/packages/visual-studio-code/) as well and then run it from the terminal by entering the command _&#8216;code&#8217;_.

Last but not least, if you&#8217;re getting started with VS Code as well, you can simplify your progress by using [Microsofts cheatsheet](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf) (this is targeting windows users, but I&#8217;m sure you&#8217;ll be fine with it).

To install the plugin, simply use CTRL + Shift + X and search for &#8216;C#&#8217;. You&#8217;ll find a package from Microsoft called _C# for Visual Studio Code (powered by OmniSharp),_ which you&#8217;ll want to install.

## Hello World!

After installing all the mentioned packages and plugins, you&#8217;re ready to start developing! You can use the built in terminal from VS Code and create a new project folder. Create a new folder wherever you want and use _dotnet new_ followed by the type of application you&#8217;d like to create.

I&#8217;ll start with a simple console application by entering _dotnet new console_. You can see all available options by entering the command without a project type.

This command generates all the neccessary files to get started. The entry point of your project will be program.cs, which is already populated with the source code for a basic _&#8216;Hello World&#8217;_ &#8211; application.

{% highlight c# %}using System;

namespace Dotnet
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hello World!");
        }
    }
}{% endhighlight %}

You can now run your program by entering _dotnet run_ inside your project folder. In this case, this will write &#8216;Hello World!&#8217; to the terminal.

One last thing I would like to cover in this post is the possibility to debug C# applications in VS Code.

The installed plugins comes with integrated intellisense and debugging, which makes it very easy to develop apps. As you may be used to from Visual Studio, you can set breakpoints by clicking on the border to the left of the row number.

To debug your application, you can use F5 or the the &#8216;Debug&#8217; entries from the menu. If that does not work, you might have the program.cs file open directly, without the project folder. In this case, open the folder in VS Code, since it contains debug configuration.