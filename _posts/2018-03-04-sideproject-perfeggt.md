---
id: 806
title: 'Sideproject: Perfeggt'
date: 2018-03-04T15:17:17+00:00
author: Marcel Jurtz
layout: post
guid: https://blog.mjurtz.com/?p=806
permalink: /2018/03/sideproject-perfeggt/
categories:
  - android
tags:
  - android
  - app
  - development
  - programming
  - sideproject
---
Once again, it&#8217;s time to announce one of my side projects: Pefeggt &#8211; A boiling time calculator for eggs. Actually, I&#8217;ve been sitting on this one for quite a while and I decided to complete a working release this weekend. The app is built for Android and it helps you to calculate the perfect time you need to boil eggs to your preferred consistency.

![Perfeggt Overview](/assets/2018/03-perfeggt-overview.png)

# Functionality & Usage

To calculate the time, I&#8217;m using [Charles D. H. Williams formula from his paper &#8220;The Science of boiling an Egg&#8221;](http://newton.ex.ac.uk/teaching/CDHW/egg/). <span>This calculation requires the weight and temperature of the egg, the height above sea level at the place where it is cooked and the desired internal temperature of the egg to determine its consistency.</span>

To provide a better user experience, the user can provide the eggs weight in grams as well as <span>the measurement in the form of sizes S (50 g.), M (58 g.), L (68 g.) and XL (75 g.). Alternatively, you can enter the weight directly. To specify the desired internal temperature, I have specified three possible selections to achieve either a soft, medium or hard result.</span>

<span>Finally, it is necessary to state the current position regarding the altitude above sea level. This determines the boiling point of the water.</span>

<span>After you have entered all the values and pressed the &#8220;<em>Calculate</em>&#8221; button, a second page will be loaded showing you the duration. Here you can now start a timer that exits the application. In the status bar you will now see a new entry indicating the time of completion. By clicking this entry, you can abort the countdown prematurely. As soon as the time has elapsed, the application will notify you with an alarm.</span>

<span>I am looking forward to any feedback! The app is available in both <a href="https://play.google.com/store/apps/details?id=com.jurtz.android.pefectegg">PlayStore</a> and Open Source on <a href="https://github.com/MarcelJurtz/Perfeggt">GitHub</a>.</span>