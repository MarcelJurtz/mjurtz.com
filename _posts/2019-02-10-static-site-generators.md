---
title: 'Static Site Generators'
date: 2019-02-10T06:00:00+00:00
author: Marcel Jurtz
layout: post
permalink: /2019/02/static-site-generators
categories:
  - web
tags:
  - blogging
  - website
  - cms
  - programming
---

Some time ago I moved this website from WordPress to Jekyll, but I never really explained the reasons. 
That's why I want to discuss in today's post the option of managing websites using an offline CMS or Static Site Generator.

CMS like WordPress are often ridiculed and devalued by their complexity or their overall structure. 
In the meantime, there is a variety of similar content management systems available, but they usually share the core functionalities and structure. 
Usually, these systems consist of a backend, via which the website is managed. Articles are stored via a relational database. 
The websites can be adapted visually to one's own preferences via templates and with the help of plug-ins a wide variety of additional functions can be implemented without major effort.

This comfort sounds very tempting, but the setup also has its disadvantages. 
Compared to a simple HTML website, considerably more resources are needed, for example a database. Plugins often load client-side scripts, 
which can significantly increase loading times. 
At first glance, CMS-based websites seem very convenient to manage and maintain, but things like system and plugin updates should not be forgotten. Ask a search engine of your choice for example "WordPress hack" and you'll see how many people don't seem to be able to do this on their own. 
Anyone without programming knowledge can set up a WordPress page, but as soon as it comes to making modifications to templates or the like, 
it becomes difficult. I don't mean to say that these systems don't have any rights to exist. 
However, the effort to set up and maintain such a system is not as trivial as you might think.

Static Site Generators, as the name suggests, rely on a different principle. 
The user usually creates his content offline and with the help of predefined routines an HTML website is created. 
This is typically done by defining templates for different elements such as posts, and then linking them to the actual content, resulting in a ready-to-use website. 
There are many systems in this area as well, one of the better known is probably Jekyll, the system I am currently using.

Jekyll is based on Ruby. I don't have any knowledge in this programming language, 
which is why I would like to briefly go through my procedure when I set up a new website with Jekyll.

First I look for an appropriate template. With GitHub you will usually find what you're looking for very quickly, 
but when you make your selection you should pay attention to the license model of the project you want to use. 
Afterwards I download the project and make adjustments according to my requirements. 
As soon as the template meets my expectations, I can start writing articles. Jekyll uses a directory called _posts, 
which contains the articles in Markdown format. With a single command I can then create my entire website and upload it to my webspace. 
I don't need a database connection and since the articles are directly available as HTML pages, I get the best possible loading times.

Enough about my personal workflow - let's discuss the advantages of systems like Jekyll. Well, it's partly obvious. 
I am independent of classic CMS solutions and need less resources on my web server. 
But especially as a software developer, the advantages of Jekyll, for example, are just beginning to take effect. 
By writing articles via Markdown, I can easily achieve a consistent style across my entire website. 
The website itself can be easily managed via a version control system like Git and a connected platform (GitHub etc.). 
The latter allows easy sharing among multiple authors. 
Features like pull requests replace the tedious administration of user profiles and authorization concepts. 
And via [GitHub Pages](https://help.github.com/articles/using-jekyll-as-a-static-site-generator-with-github-pages/) you can even host your Jekyll website directly via GitHub. 
The text-based article management allows easy versioning and makes it much easier for you to convert your articles if you want to switch to another system.

Static websites are in general very easy to deploy and changes can be submitted almost instantly. 
Since no underlying databases are required, the website can be scaled very well (e.g. via AWS or Netlify). 
Due to the minimal requirements, the webspace can of course also be managed more cost-effectively. 
As soon as the website is online, there is virtually no need for server-side maintenance. 
And of course you can simply write your own tools that implement various features of your website before it is being published. 
For example, automated checking for links that are no longer available, etc.

As always, no general choice can be made between classic CMS and Offline-CMS / Static Site Generator. 
It is important to weigh up the needs of the users and derive the criteria for the software from this. 
Especially in the area of blogging I can at least recommend Jekyll to you personally. 
You'll have to invest some time to get used to it, but you'll find your way around quickly.

If you decide to switch - most CMS have export plugins for your articles, so you can easily convert them to the new format. Good luck! 