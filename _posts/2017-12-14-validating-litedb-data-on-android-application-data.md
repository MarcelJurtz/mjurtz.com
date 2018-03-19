---
id: 652
title: Validating LiteDB Data from an Android App
date: 2017-12-14T21:00:28+00:00
author: Marcel Jurtz
layout: post
guid: https://blog.mjurtz.com/?p=652
permalink: /2017/12/validating-litedb-data-on-android-application-data/
categories:
  - Tips
tags:
  - Android
  - App
  - Development
  - Tools
---
In my new sideproject CryptoFolio, which I announced in [this article](https://blog.mjurtz.com/2017/12/sideproject-cryptofolio/), I described that I&#8217;d like to use LiteDB to locally store data that I received from the coinmarketcap-API. LiteDB is basically a NoSQL database, in particular a document store. It functions on a serverless file basis and can be accessed via a simple API.

While it is quite easy to implement the databases functionality itself, testing the resulting data structure is somewhat more difficult. But to be sure that my data is saved correctly, I needed a way to explore the database located on the android device. Therefore, I use the following approach:

I start by downloading [LiteDB Explorer](https://github.com/JosefNemec/LiteDbExplorer), a free tool to visually inspect LiteDB database files. However, to view such a file, I need access to it first.

You can do this by using the Android Debug Bridge (ADB). After enabling the developer options on your device, you can use the command <code class="EnlighterJSRAW" data-enlighter-language="generic">adb backup -noapk com.your.packagename</code> to copy your applications data from the smartphone to the pc. Note that you can use <code class="EnlighterJSRAW" data-enlighter-language="generic">adb restore backup.db</code> to go the opposite way, which can be useful for restoring application backups.

After entering the command, you&#8217;ll have to confirm the operation on the device.  The backup will be copied to your home directory per default. As a result of this operation, you will receive an .ab-file. I recommend using ADB Backup Extractor, which you can download from [SourceForge](https://sourceforge.net/projects/adbextractor/files/latest/download) for free. This tool converts the .ab-file to a regular .tar-file, which can easily be extracted using a tool like 7-Zip.

Use this command to convert your ab file:

<pre class="EnlighterJSRAW" data-enlighter-language="generic">java -jar abe.jar [-debug] [-useenv=yourenv] unpack <backup.ab> <backup.tar> [password]{% endhighlight %}

Among the unzipped files you will now find the database file. Use the previously mentioned explorer to visualize it.