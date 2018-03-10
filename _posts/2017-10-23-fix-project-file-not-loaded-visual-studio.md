---
id: 454
title: 'Fix &#8220;project file could not be loaded&#8221; in Visual Studio'
date: 2017-10-23T06:44:05+00:00
author: Marcel Jurtz
layout: post
guid: http://blog.mjurtz.com/?p=454
permalink: /2017/10/fix-project-file-not-loaded-visual-studio/
categories:
  - Tips
---
I recently stumbled upon the problem that my solution in Visual Studio (2017) could not load all of my projects. Since the solution of the problem took a while, I decided to document my approach here.

No configuration of the project has been changed, which is why I have no clue why things are fucked up. But hey, never underestimate the ability of microsoft to disappoint people. Anyway, I found the following solution working for me: First, delete all .suo files you can find and then rebuild the project. That was enough for me to get the project up and running again.