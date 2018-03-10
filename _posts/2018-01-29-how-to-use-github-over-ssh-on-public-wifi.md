---
id: 743
title: Using GitHub over SSH on public WIFI
date: 2018-01-29T06:00:11+00:00
author: Marcel Jurtz
layout: post
guid: https://blog.mjurtz.com/?p=743
permalink: /2018/01/how-to-use-github-over-ssh-on-public-wifi/
categories:
  - Tools
tags:
  - Git
  - Linux
  - Tools
---
When using GitHub via SSH, port 22 will be used per default. However, often times you&#8217;ll see this port being blocked on public WIFI networks. On a Linux machine, there is a simple way to change the default port.

To test if our plan works out, you can use the following command:

<pre class="EnlighterJSRAW" data-enlighter-language="no-highlight">ssh -T -p 443 git@ssh.github.com</pre>

This will test the ssh connection by using port 443. If this returns a positive result, you can change the port in your ssh config file.

You can find the SSH configuration in _~/.ssh/config_. To change the port, open the file and add a new entry for github, which will look like this:

<pre class="EnlighterJSRAW" data-enlighter-language="no-highlight">Host github.com
  Hostname ssh.github.com
  Port 443</pre>

You can use whatever port you&#8217;d like. I&#8217;m using port 443, which is the default HTTPS port. That is also the one GitHub recommends to use in [their documentation](https://help.github.com/articles/using-ssh-over-the-https-port/). Save your changes and you will be able to work as you&#8217;re used to.