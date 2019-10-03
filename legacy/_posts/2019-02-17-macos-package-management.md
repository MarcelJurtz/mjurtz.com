---
title: 'MacOS Package Management'
date: 2019-02-17T06:00:00+00:00
author: Marcel Jurtz
layout: post
permalink: /2019/02/macos-package-management
categories:
  - macos
tags:
  - tools
  - apple
  - mac
  - software
---

The ability to work with the terminal under MacOS and use my familiar Linux workflow is awesome, but I have always missed a crucial component: a package manager.

Homebrew is a package manager for MacOS, according to the developers "The missing package Manager for macOS (or Linux)". Today I would like to explain how you install and use Homebrew productively.

First of all: You can find the project's website [here](https://brew.sh), there you will find all the information and documentation you could wish for.

You might have to install the command line tools first. This can be done with the command xcode-select --install. If you already have Xcode or other software that needs the tools installed, you can skip this step. You can use xcode-select -p to check if the Command Line Tools are already installed.

To initialize the installation, use the command 
{% highlight bash %}
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
{% endhighlight %}

Homebrew is based on Ruby, the version included in MacOS is sufficient for use, you don't need to install anything additional at this point.

Note: If you want to uninstall Homebrew, you can use the same command and replace the install at the end of the URL with uninstall.

You can use the _brew search_ command to search for available packages. Without any additional parameters, all packages will be listed. This overview can also be found on the [website](https://formulae.brew.sh/formula/). If you want to search for a specific package, you can simply append your search term to the above command. Once you have found a package, you can retrieve information about it using _brew info [packagename]_. Alternatively, you can use _brew list_ to display only the packages you have installed.

Packages can now be installed using _brew install [packagename]_ and uninstalled using _brew uninstall [packagename]_. You can also uninstall packages using rm or remove besides uninstall.

You can already work with this state, but there are still no possibilities to update brew yourself, update package lists and update installed software.

The first is done with the _brew update_ command. The command _brew upgrade_ is then used to update the packages. This command updates all available packages. Alternatively, you can upgrade a single package using the _brew upgrade [packagename]_ command. And by the way: _brew outdated_ lists all obsolete packages. To exclude packages from the upgrade process, you can use the _brew pin [packagename]_ command, or _brew unpin [packagename]_ to undo this. Pinned packages are not updated automatically.

This should give you a good introduction to homebrew and make managing your software easier. You can also find a complete overview of the commands on the [project website](https://docs.brew.sh/Manpage).