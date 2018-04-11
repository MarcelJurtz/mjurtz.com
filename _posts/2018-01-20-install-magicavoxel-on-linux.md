---
id: 729
title: Installing MagicaVoxel on Linux
date: 2018-01-20T06:00:57+00:00
author: Marcel Jurtz
layout: post
guid: https://blog.mjurtz.com/?p=729
permalink: /2018/01/install-magicavoxel-on-linux/
categories:
  - tools
tags:
  - design
  - gamedev
  - linux
  - software
  - tools
  - windows
---
As a programmer with definite lack of visual skills, [MagicaVoxel](http://ephtracy.github.io/) is a gift from God to create usable 3D models. Unfortunately, there is no installation option for Linux on the official website. With the help of Wine it is possible to use MagicaVoxel under Linux. And it really works great!

## Installation Prerequisites

As I said you need to run [Wine](https://www.winehq.org/). As stated on their landing page, Wine is a compatibility layer that allows Windows applications to run under POSIX-compliant operating systems such as Linux, macOS and BSD. Instead of simulating internal Windows logic, such as a virtual machine or emulator, Wine translates Windows API calls into real time POSIX calls, eliminating the performance and memory degradation associated with other methods. Wine allows you to integrate Windows applications neatly into your desktop environment.

You should be able to install wine using your distros package manager. On Linux Mint, that&#8217;s _sudo apt install wine_. Hint: If you&#8217;re on arch, you can skip this step. There&#8217;s an [AUR package](https://aur.archlinux.org/packages/magicavoxel/) available (It&#8217;s also based on wine).

## Installing MagicaVoxel

After installing wine, you can run .exe files from your terminal by entering _wine ~/path/to/exe_. All you have to do is download the windows version and store it somewhere save. In my setup, I use a folder in my home directory which contains all the files from the windows release. So, for me it&#8217;s _wine ~/MagicaVoxel/MagicaVoxel.exe_. You can also use this command to create desktop shortcuts.

Of course, you&#8217;re not limited to MagicaVoxel, wine is capable of so much more. But never having it touched before, it really makes a great impression.