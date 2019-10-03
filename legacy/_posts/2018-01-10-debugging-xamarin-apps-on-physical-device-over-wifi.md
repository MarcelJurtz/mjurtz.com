---
id: 715
title: Xamarin Debugging over WIFI
date: 2018-01-10T06:00:31+00:00
author: Marcel Jurtz
layout: post
guid: https://blog.mjurtz.com/?p=715
permalink: /2018/01/debugging-xamarin-apps-on-physical-device-over-wifi/
categories:
  - android
tags:
  - dotnet
  - android
  - app
  - csharp
  - development
  - programming
  - tools
  - windows
  - xamarin
---
Xamarin utilizes the Android Debug Bridge (ADB) for debugging on physical devices, which are usually connected over usb. While this feature definitely is useful, it would sometimes be more practical if you could do this over WIFI instead.

Luckily, that is possible! You can simple switch between using USB and TCP/IP for debugging. To get started, you need to connect your device via USB to the computer you&#8217;re debugging from. I&#8217;m using Windows, so the commands I&#8217;ll execute the following commands from the commandline (which runs as an administrator), but since they are adb commands, they should be the same on Linux or macOS.

The first command to switch from USB to TCP/IP is <code class="EnlighterJSRAW" data-enlighter-language="generic">adb tcpip <port></code>. Replace <port> with anything you like, I&#8217;ll use the same as Xamarin suggests in their [documentation](https://developer.xamarin.com/guides/android/getting_started/installation/set_up_device_for_development/), which is 5555. After this is done, you&#8217;ll want to connect adb to your device. To do this, you need its IP address, which can be discovered by going to _Settings_ &#8211; _WIFI_ &#8211; _More_ (the dots-symbol in the upper right corner) &#8211; _Advanced_.

After you obtained the IP, connect to the device using adb connect <code class="EnlighterJSRAW" data-enlighter-language="generic"><ip>:<port></code>. After this step, you can disconnect the device from USB. When using <code class="EnlighterJSRAW" data-enlighter-language="generic">adb devices</code>, this should now list a device at the matching IP address. Visual Studio will now also display the devices IP address in the debugging window.

To switch back from WIFI to USB, simply use <code class="EnlighterJSRAW" data-enlighter-language="generic">adb usb</code>.

Since this is all based on ADB, debugging over WIFI is not restricted to use with Xamarin & Visual Studio. You can also use this for regular Android apps that are developed with Android Studio!

&nbsp;