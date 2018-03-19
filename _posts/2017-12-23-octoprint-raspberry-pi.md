---
id: 690
title: Octoprint on a Raspberry Pi
date: 2017-12-23T15:44:40+00:00
author: Marcel Jurtz
layout: post
guid: https://blog.mjurtz.com/?p=690
permalink: /2017/12/octoprint-raspberry-pi/
categories:
  - Hobby
tags:
  - 3D-Printing
  - Linux
  - Raspberry Pi
---
Being tired of running back and forth to my 3D-printer to change stl files and starting / stopping prints, I decided to give [Octoprint](https://octoprint.org/) a shot. Octoprint basically provides a webinterface for 3D-printers, which allows you to send tasks over your local network to the printer and to monitor the prints. The software also provides a plugin functionality, so you can create your custom addons if required.

Note that you can use Octoprint for slicing, but in my case I will be starting of with only sending pre sliced stl files to print.

## Hardware & Installation

In this article, I cover the usage in combination with a Raspberry Pi. If you have a spare Raspberry Pi 3, I recommend using it, since it has built in WIFI. However, you can of course also use older versions with wired connection or by adding a WIFI adapter.

If you don&#8217;t have anything installed on your pi, you can use [OctoPi](https://github.com/guysoft/OctoPi), which is a preconfigured image which includes Octoprint. However, since I already have raspbian installed, I&#8217;ll install Octoprint manually.

As you can see in the [installation guide](https://github.com/foosel/OctoPrint/wiki/Setup-on-a-Raspberry-Pi-running-Raspbian#basic-setup), you&#8217;ll first need to install several packages for python. Of course, update your system first.

<pre class="EnlighterJSRAW" data-enlighter-language="null">sudo apt-get install python-pip python-dev python-setuptools python-virtualenv git libyaml-dev build-essential{% endhighlight %}

As soon as this is done, clone the repository from GitHub and run the installation script.

<pre class="EnlighterJSRAW" data-enlighter-language="null">git clone https://github.com/foosel/OctoPrint.git
cd OctoPrint
virtualenv venv
./venv/bin/pip install pip --upgrade
./venv/bin/python setup.py install
mkdir ~/.octoprint{% endhighlight %}

After installing, you might have to update user configuration:

<pre class="EnlighterJSRAW" data-enlighter-language="null">sudo usermod -a -G tty pi
sudo usermod -a -G dialout pi{% endhighlight %}

Now, you should be able to start the service:

<pre class="EnlighterJSRAW" data-enlighter-language="null">~/OctoPrint/venv/bin/octoprint serve{% endhighlight %}

You should be able to connect to the web interface by using the pi&#8217;s IP on port 5000. After your first connect, you&#8217;ll face an installation page. These steps are useful if you want to setup custom slicing for octoprint. Since I slice my models in cura and send stl files to the printer, I&#8217;ll skip these steps.

## Customization

I currently don&#8217;t have a webcam for my pi, so I won&#8217;t install webcam support. However, you can check out the instructions [here](https://github.com/foosel/OctoPrint/wiki/Setup-on-a-Raspberry-Pi-running-Raspbian#webcam) if you&#8217;d like to do this.

I&#8217;d like to set up my installation to automatically start when the pi boots up. These steps are also covered in the [installation guide](https://github.com/foosel/OctoPrint/wiki/Setup-on-a-Raspberry-Pi-running-Raspbian#automatic-start-up), but I&#8217;ll wrap them up. Start by copying the init script and adjusting its privileges to be executable:

<pre class="EnlighterJSRAW" data-enlighter-language="null">sudo cp ~/OctoPrint/scripts/octoprint.init /etc/init.d/octoprint
sudo chmod +x /etc/init.d/octoprint
sudo cp ~/OctoPrint/scripts/octoprint.default /etc/default/octoprint{% endhighlight %}

Remove the comment-symbol (#) from the DAEMON-line in your <code class="EnlighterJSRAW" data-enlighter-language="null">/etc/default/octoprint</code> file and add the script to your autostart with <code class="EnlighterJSRAW" data-enlighter-language="null">sudo update-rc.d octoprint defaults</code>.

Besides having automatic startup enabled, you can now start, stop and monitor the octoprint service with <code class="EnlighterJSRAW">sudo service octoprint [start / stop / restart / status]</code>.

## Connecting to a 3D-Printer

Well, that last step is quite simple. In my case, my Anet A8 came with a USB type B cable which can be direcly plugged into the Raspberry Pi. And that&#8217;s it. After connecting to the printer, you can&#8217;t just only upload files and print them, you&#8217;re also able to see the files available on the printers sd card and restart older jobs.

## Closing Thoughts

While that first introduction gave me some insights on what octoprint is capable of, but I&#8217;ve definitely barely scratched the surface. One cool thing for example is the built in API, which allows you to connect for example Android apps to monitor temperatures or to upload files.

All in all, I&#8217;ve had a great start with Octoprint and really recommend trying it out. Stay tuned for future updates!

&nbsp;

&nbsp;