---
id: 461
title: P2P Synchronization with Syncthing
date: 2017-11-01T13:12:23+00:00
author: Marcel Jurtz
layout: post
guid: http://blog.mjurtz.com/?p=461
permalink: /2017/11/p2p-synchronization-with-syncthing/
categories:
  - Productivity
  - Tools
tags:
  - Android
  - App
  - Network
  - P2P
  - Syncthing
  - Windows
---
Everyone has different programs and tools that they particularly appreciate. One of these programs is syncthing for me, so I will briefly discuss how it works and how it is set up in this article. Syncthing is used to synchronize files between different devices and also supports versioning of these files. [Syncthing is available for Windows, Android and as cross-platform wrapper](https://syncthing.net/). I personally use it on Windows 10,  [Arch Linux](https://wiki.archlinux.org/index.php/Syncthing) and Android (via [Playstore](https://play.google.com/store/apps/details?id=com.nutomic.syncthingandroid) or [FDroid](https://f-droid.org/en/packages/com.nutomic.syncthingandroid/)). It is completely open source and available on [GitHub](https://github.com/syncthing). For general problems, I recommend reading [their FAQ](https://docs.syncthing.net/users/faq.html).

Syncthing transfers files over the internet, but if you use it for local transfers only, it will be restricted to LAN and won&#8217;t use the internet. However, you can also [host your own relay server ](https://docs.syncthing.net/users/strelaysrv.html)and contribute to the network, which is organized decentrally.

In the following, I&#8217;ll be setting up a shared folder to synchronize my playlist I use for running, since my main smartphone is a Xiaomi Redmi Note 4, but since my nexus 5 is more practical by weight and size, I use this to run. My setup can be summarized as follows:

  * Xiaomi Redmi Note 4 (Android)
  * Google Nexus 5 (Android)
  * MacBook Air (Arch Linux)
  * Desktop-PC (Windows 10)

## Setting up and Connecting Devices

On windows and linux, the graphical user interface of syncthing is started via browser (localhost, default port is 8384). You can set a name for your device, which is used as default on connecting devices later. Each device is uniquely addressable by an id which can be accessed via Actions &#8211; Show ID. You can add a device by clicking _Add Remote Device_ in the lower right corner. Devices that are located in the same network are recognized automatically, otherwise they can be added by entering their id.

<img class="aligncenter size-medium wp-image-465" src="https://i2.wp.com/blog.mjurtz.com/wp-content/uploads/2017/11/1-500x268.png?resize=500%2C268" alt="Syncthing Overview" width="500" height="268" srcset="https://i1.wp.com/blog.mjurtz.com/wp-content/uploads/2017/11/1.png?resize=500%2C268&ssl=1 500w, https://i1.wp.com/blog.mjurtz.com/wp-content/uploads/2017/11/1.png?w=1167&ssl=1 1167w" sizes="(max-width: 500px) 100vw, 500px" data-recalc-dims="1" />

When adding devices, there are multiple options available. One of these is to mark a device as _Introducer_. This allows to forward all devices that are available on the remote device to the current one, which is great for using Syncthing on a NAS-like server.

## Sharing Folders

After networking the devices with each other, directories can be created which will then be synchronized. To do this, simply click _Add Folder_ on the web interface. On Android, this can be done by clicking the plus-button:

<img class="aligncenter size-medium wp-image-466" src="https://i1.wp.com/blog.mjurtz.com/wp-content/uploads/2017/11/Screenshot_20171101-124423-360x640.png?resize=360%2C640" alt="Syncthing Android Main" width="360" height="640" srcset="https://i2.wp.com/blog.mjurtz.com/wp-content/uploads/2017/11/Screenshot_20171101-124423.png?resize=360%2C640&ssl=1 360w, https://i2.wp.com/blog.mjurtz.com/wp-content/uploads/2017/11/Screenshot_20171101-124423.png?w=1080&ssl=1 1080w" sizes="(max-width: 360px) 100vw, 360px" data-recalc-dims="1" />

You&#8217;ll want to specify the path of the folder, as well as the label, which will be displayed on all devices later. Since I&#8217;ll be using this folder for mp3-files, I won&#8217;t enable versioning for it, but you can select one of multiple options at this point. Finally it is specified for which of the available devices the directory should be available. After completing the setup, the devices that were selected will automatically receive a request to accept the shared folder. By doing this, a local folder needs to be set again. I&#8217;ve encountered minor problems here, where the shares won&#8217;t be recognized automatically on Android devices. I&#8217;ve solved this problem by switching to the Web GUI via settings. By using the web interface, you have the additional option of specifying the scanning interval and more (see _advanced settings_).

<img class="aligncenter size-medium wp-image-469" src="https://i2.wp.com/blog.mjurtz.com/wp-content/uploads/2017/11/Screenshot_20171101-135001-360x640.png?resize=360%2C640" alt="Syncthing New Folder" width="360" height="640" srcset="https://i1.wp.com/blog.mjurtz.com/wp-content/uploads/2017/11/Screenshot_20171101-135001.png?resize=360%2C640&ssl=1 360w, https://i1.wp.com/blog.mjurtz.com/wp-content/uploads/2017/11/Screenshot_20171101-135001.png?w=1080&ssl=1 1080w" sizes="(max-width: 360px) 100vw, 360px" data-recalc-dims="1" />

After setting up everything correctly, synchronization should begin automatically. And that&#8217;s basically it! You can set up multiple folders for different devices and multiple filetypes to fit your individual needs.

<img class="aligncenter size-medium wp-image-470" src="https://i2.wp.com/blog.mjurtz.com/wp-content/uploads/2017/11/4-500x421.png?resize=500%2C421" alt="Synchting Synchronization Progress" width="500" height="421" srcset="https://i1.wp.com/blog.mjurtz.com/wp-content/uploads/2017/11/4.png?resize=500%2C421&ssl=1 500w, https://i1.wp.com/blog.mjurtz.com/wp-content/uploads/2017/11/4.png?w=567&ssl=1 567w" sizes="(max-width: 500px) 100vw, 500px" data-recalc-dims="1" />