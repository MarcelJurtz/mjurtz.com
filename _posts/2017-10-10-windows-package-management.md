---
id: 426
title: Windows Package Management
date: 2017-10-10T11:58:04+00:00
author: Marcel Jurtz
layout: post
guid: http://blog.mjurtz.com/?p=426
permalink: /2017/10/windows-package-management/
categories:
  - Tools
tags:
  - Software
  - Tools
  - Windows
---
As a Windows user with Linux knowledge there are several things under Linux that I would like to have on for Windows. Since a complete switch from Windows to Linux is not feasible for me due to different requirements, I looked for alternatives. An important component of various Linux distributions is their package manager. This article discusses the implementation of such a feature under Windows and shows how it can be used.

While researching this topic I came across chocolatey, a CLI-tool that acts as package manager for Windows operating systems. Chocolatey is open source, [the code is available on GitHub](https://github.com/chocolatey). On the [project&#8217;s website](https://chocolatey.org/install) you will also find instructions for installation, which can be performed using CMD commands or via powershell.

## Install, Update, Uninstall

A list of available packages can be viewed on <https://chocolatey.org/packages>. As with the Arch User Repository, packages are managed by users, which means that there is a very large selection of available packages. The pattern for installing packages is 

> choco install <packagename></code>

A full list of commands can be found in [the docs](https://chocolatey.org/docs). Installed packages can be uninstalled using 

> choco uninstall <packagename></code> 

or upgraded by using 

> choco upgrade<packagename></code>

If you want to upgrade all installed packages, simply use the keyword _all_ instead of a specific packagename.

By default, Chocolatey creates a directory under _C: \ProgramData_ which contains the program files. The installation locations for the individual programs depend on the configuration of the package, but usually either _C:\Program Files_ or _C:\Program Files (x86)_ is used.

## Further Commands

Of course, the package management does not only include installation, upgrade and deinstallation of packages, so this section is dedicated to some other features. Check out the docs for a complete list of features.

You can search packages by using

> choco search <packagename></code>

which results into something similar to the following listing.

{% highlight DOS %}
D:\>choco search jenkins
Chocolatey v0.10.8
jenkins 2.60.3 [Approved]
gradle 4.1.0 [Approved] Downloads cached for licensed users
notify-me-ci 1.3.3.13 [Approved] Downloads cached for licensed users
JenkinsOnDesktop 1.0
4 packages found.
{% endhighlight %}

To check out the installed version of choco, you can use

> choco --version</code> 

or 

> choco -v

To upgrade chocolatey, just use the regular update command. To list all packages available, use 

> choco list

If you only want to receive the packages that are installed locally, add the _-l_ flag.

These few commands should be sufficient to enable you to get started with package management under Windows. As already mentioned, the documentation of the project conveys all further details in a simple and understandable way. And, if you&#8217;re not comfortable using the command line, there is also a graphical user interface for chocolatey available, which can be found under the name _chocolateygui_.