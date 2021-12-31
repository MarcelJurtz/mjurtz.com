---
title: 'Password-Synchronization with Syncthing'
date: 2021-11-25T06:00:00+00:00
author: Marcel Jurtz
layout: post
permalink: /2021/11/password-synchronization-with-syncthing
categories:
  - security
tags:
  - security
  - tools
---

If you are reading this, chances are you already use a password manager. I have been doing that for several years now. But I have to admit that my password manager has only run on my phone so far and I have always transferred passwords that I use on my PC by hand. Probably out of sheer laziness. Today, I use this blog post to finally synchronise my password database between any number of devices - and to show you how to do it. The whole thing not only makes dealing with passwords more convenient, but naturally also reduces the inhibition threshold to actually use the applications. This in turn improves the overall security of your accounts.

Let's briefly talk about the software: I use Keepass 2 on my PC and Keepass2Android on my mobile phone. This article should work with all applications that use kdbx files. To synchronise the individual files, I use Syncthing, which is available for various platforms. Accordingly, this article is not limited to Windows and Android.

## Syncthing

Syncthing is an [open source application](https://github.com/syncthing/syncthing) that can be used to synchronise files between devices. The software supports synchronization via local and remote networks. As mentioned earlier, syncthing is available for a lot of different platforms, check out their [download-page](https://syncthing.net/downloads/) for details.

Syncthing is implemented in a peer-to-peer manner, which means that files are synchronised between the devices without a central server. Communication is protected via [TLS](https://de.wikipedia.org/wiki/Transport_Layer_Security) and [Perfect Forward Secrecy](https://de.wikipedia.org/wiki/Perfect_Forward_Secrecy).

Syncthing allocated a unique ID to each device, which is used for address resolution, authentication and authorisation. In order for two devices to communicate with each other, they must know the respective device ID. If the IDs are known to each other, the devices are located using several discovery protocols:

* **Local Discovery**: Broad- or multicast packets are sent into the local network via the router at intervals of 30 to 60 seconds. Each participating device therefore sends so-called announcement packets at regular intervals and keeps a table with all received announcements. Even in the event of an IP address change within a network, this ensures that the devices know the current IP address of all participants.
* **Global Discovery**: If two devices are not in the same network, the global discovery protocol is used. This consists of two steps: First, each device periodically sends an announcement packet to a global discovery server. In the second step, each device sends a query packet with the (known) device ID of its counterpart. If the Syncthing server knows the device ID and the current location or IP address, it responds with the announcement packet.

Syncthing automatically starts the synchronization as soon as the location of the participating devices is known. If the devices cannot communicate directly with each other, e.g. due to firewall restrictions, a connection using a relay server is established. Syncthing hereby automatically detects whether a relay server is necessary or not. If you want to use syncthing only in your local network, you can also turn off the (remote) discovery protocols.

One last thing to mention is that syncthing doesn't perform a *real* synchronization. It's more of a *push* to other devices after recent changes.

## Setup

To get started, use the [download-page](https://syncthing.net/downloads/) to get syncthing for your respective platform(s). I am going to use Android and Windows 10 for this post.
For Windows, just unzip the downloaded folder and run the contained executable. You'll be greeted with a web interface that shows you your installation details and let's you the configure your setup.

After installing, you'll want to connect your devices. To do so, I use my phone to scan the QR-Code on my PC. You can do this both ways, but might have to type in the id if you have no camera available. When you have the devices on the same local network, syncthing can discover devices automatically. On the second device, you will now receive a notification that the first device would like to establish a connection. As soon as you have agreed to this, you can continue with the configuration.

Synthing works with the concept of directories. A virtual directory is created for each physical folder I want to synchronize between devices. The virtual folders are displayed in the web interface. I start by creating a "Keepass" folder on my Android device and link it to the file path I want to use. I then select my PC to share the folder with it. After saving, I receive a new notification on my PC to include the shared folder. I confirm this too and now configure the physical part of my second device.

Now the two directories are linked, and as long as both devices are online, Syncthing will keep their contents up to date. It also displays the current status, which is currently "Out of sync". Shortly afterwards, syncthing starts the first adjustment and my Keepass file already ends up on the PC.

## Conclusion

Syncthing is a fast, simple and very flexible solution to synchronise files between multiple devices. This also works very well with Keepass databases, but care must be taken here regarding merge conflicts. You should therefore avoid changing data on both databases while both devices are not connected. If necessary, Keepass itself has a merge function that can be used to synchronise database statuses that have drifted apart.