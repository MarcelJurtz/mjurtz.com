---
title: 'Bypassing Microsofts Windows Media Creation Tool'
date: 2018-04-09T18:00:00+00:00
author: Marcel Jurtz
layout: post
permalink: /2018/04/bypass-windows-media-creation-tool/
categories:
  - windows
tags:
  - tools
  - windows
---

Remember when you could simply download official Windows ISO images to burn on DVDs and install? 
Microsoft now uses this thing called 'Media Creation Tool' to download ISOs, but there's a simple way to bypass this and download actual images.

When searching for Windows10 ISOs, you'll likely find this url, which provides a download for the Media Creation Tool.

* [https://www.microsoft.com/de-de/software-download/windows10](https://www.microsoft.com/de-de/software-download/windows10)

But: there's also this url

* [https://www.microsoft.com/de-de/software-download/windows10ISO](https://www.microsoft.com/de-de/software-download/windows10ISO)

However, when browsing this site on a windows machine, it automatically redirects you to the first of the pages I linked.
If you're not using Windows, you'll be able to use the second url to download the ISO file directly.
To be able to this on Windows as well, you'll simply need to fake your browsers user agent header to make Microsoft think you're using Linux (for example).

That's also pretty easy to achieve, there are many browser plugins available for this very purpose. 
Just search "user agent switcher" in your addon repository and you're good to go!


