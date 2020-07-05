---
title: 'Understanding REM in CSS'
date: 2020-07-05T06:00:00+00:00
author: Marcel Jurtz
layout: post
permalink: /2020/07/understanding-rem-in-css
categories:
  - csharp
tags:
  - css
  - development
  - web
---

If you're working in any kind of ui-related software development, you're probably familiar with several units in which elements are measured and defined. Usually, I have to dive into the topics whenever I'm working with a new unit, so I figured I just write it down for my future self. Todays unit: REM in CSS.

Let's skip the dad-jokes on name-related american rockbands and jump straight into action by looking at the ![w3c specification](https://www.w3.org/TR/css-values-3/#font-relative-lengths).

Looking at this rem belongs to the font-related units and 1rem equals the fontsize of the html element.
You might be familiar with the unit *em*, which is relative to its parent element. Rem on the other hand is only relative to the root element. This helps you organize your css files and not get overwhelmed with different size definitions.