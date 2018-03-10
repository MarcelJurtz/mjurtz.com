---
id: 794
title: Introduction to i3
date: 2018-02-28T06:00:57+00:00
author: Marcel Jurtz
layout: post
guid: https://blog.mjurtz.com/?p=794
permalink: /2018/02/introduction-to-i3/
categories:
  - Linux
tags:
  - Efficiency
  - Linux
  - Software
---
In today&#8217;s article I would like to give you an insight into the usage of a tiling window manager, which I will illustrate with the example of i3.

A tiling window manager divides the screen into non-overlapping areas. You are probably more used to a stack-based system where windows can overlap. However, the concept of avoiding overlapping offers a major advantage: Due to the pure operation of the computer via keyboard, it is possible to work at a faster pace after a short training period.

<img src="https://i1.wp.com/blog.mjurtz.com/wp-content/uploads/2018/02/i3_screenshot.png?resize=750%2C469&#038;ssl=1" alt="i3 showcase with multiple windows" width="750" height="469" class="aligncenter wp-image-799" srcset="https://i1.wp.com/blog.mjurtz.com/wp-content/uploads/2018/02/i3_screenshot.png?resize=500%2C313&ssl=1 500w, https://i1.wp.com/blog.mjurtz.com/wp-content/uploads/2018/02/i3_screenshot.png?w=1440&ssl=1 1440w" sizes="(max-width: 750px) 100vw, 750px" data-recalc-dims="1" />

Of course, this is not everyone&#8217;s cup of tea, but I recommend you to give it a try. The Window Manager can be installed in parallel to your existing one and can be selected during the login process. Also, the installation doesn&#8217;t require much disk space (~2 MB).

## Installation & Setup

I&#8217;m using Arch in combination with the MATE desktop environment. My current window manager is marco, MATEs default which is based on Metacity. On Arch Linux you can install i3 via the package _i3_ in the official package sources. This should be the same for most other package managers, but if necessary consult the search engine of your choice for your particular case.

After installing, you only need to log out and then log in again with the new Window Manager. You can usually define that using the small gear symbol. Of course, you can also set i3 directly as the default WM, but for testing purposes I prefer the temporary setup.

## Getting Started

It&#8217;s time to get our hands dirty. After you have successfully logged in, you will be greeted by a simple screen with no content except for some status indicators at the bottom. On the first start you get to choose wether you want to create your own configuration or use the standard setup. I will work with the standard first.

i3 is based on the use of a so-called mod key. This provides the basis for a large number of keyboard combinations. By default, either the Windows or alt key is used for this purpose.

As promised, everything is now controlled by keyboard combinations. Let&#8217;s begin with the basic functions.

  * To create a new window, use _Mod + Enter_
  * To close the current window, use _CTRL + d_
  * Per default, windows will be added horizontally. To switch to vertical mode, use _Mod + v_
  * Use _Mod + h_ to switch back to horizontal mode

To switch between open windows, you can use the following commands:

  * _Mod + j_ to move left,
  * _Mod + ;_ to move right
  * _Mod + l_ to move up
  * And _Mod + k_ to move down

The last shortcuts I&#8217;d like you to know are _Mod + <number>_, where you can replace <number> by any number you want to switch between multiple screens. By pressing _Shift_ additionally, you can move the current window to the specified screen.

## Conclusion & Further Steps

While I think that these few commands will help you to get started with i3, there are far more available. I definitely recommend you to check out [i3s official documentation](https://i3wm.org/docs/), it&#8217;s great. And as always, just try everything out and have fun!