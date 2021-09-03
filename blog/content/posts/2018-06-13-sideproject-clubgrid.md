---
title: 'Sideproject - ClubGrid'
date: 2018-06-13T06:00:00+00:00
author: Marcel Jurtz
layout: post
permalink: /2018/06/sideproject-clubgrid
categories:
  - csharp
tags:
  - csharp
  - web
  - development
  - programming
  - sideproject
  - asp.net
---

It is finally time to announce my latest side project. ClubGrid was originally designed to make it easier for my coach to manage our baseball games. However, the result can be used for clubs of any kind and I hope that I can also help others with my software.

The problem we had was the organization of games and events. Some players posted their acceptance or rejection via Whatsapp, others via Facebook, some via email and others personally. This made it quite complicated for us to keep track of who is present where and when.

In addition, there was the assignment of duties. Players alone are not enough, additional umpires, scorers, coaches and people who take care of the sales are needed.

This finally gave me the idea to solve the problem via a portal, which can be integrated into the existing club website. Team members can register there for events and trainers can see at a glance who is participating and where they would like to help.

The plan was born. I have some knowledge of PHP and therefore decided to implement the platform with this technology. I started with ER modeling. Then I derived my individual pages from this model and created wireframes for the rough design of the website. And then things were ready to start!

As I said, I started to implement the website in PHP. That worked quite well, even though it seemed rather messy to me. For the time being, I accepted that and continued to work on my prototype. After some time (I had almost finished the features of the MVP), I nevertheless decided to change technology.

There were several reasons for my decision. The first obvious thing was the lack of transparency in my source code. But I also didn't want to limit myself to my basic PHP knowledge. However, since I had no real interest in delving deeper into PHP and wanted to strengthen my knowledge in other areas instead, I decided to implement the application in C#. At that time I had heard of ASP.NET, but that was all. I never used it. So I read the basics and quickly switched to a Pluralsight course to get my hands dirty with a real example.

I don't want to say much about ASP.NET (Core) here, but compared to my PHP version I'm thrilled. The code is much better structured, clearer and I was able to make much faster progress.

Another possibility I took during the development was the use of postgres as the underlying database. Besides the regular functions I haven't played much with it yet, but so far I have a very positive impression. 

But enough about that. Let's look at the result:

ClubGrid is my first real web application. The tech stack of the software consists of ASP.NET Core and PostgreSQL, the connection is made using Entity Framework Core. The software offers clubs the possibility to manage their events internally and thus enables trainers to see the status of the promises at a glance. Originally, I only planned to release the software for my local baseball club, but the software can quickly be extended to support other sports or other clubs in general. [I setup a website for showcasing the application](https://clubgrid.mjurtz.com).

In the next days I will implement the last missing functionalities, so that we can put the first instance live. Then I'd like to make some surface improvements and add support for more sports. 

As soon as all this has happened, I would like to include several options for notifications. I am very curious about what will follow and look forward to any feedback or suggestions for new features! 