---
title: 'Introduction to VCS'
date: 2018-06-23T06:00:00+00:00
author: Marcel Jurtz
layout: post
permalink: /2018/06/introduction-to-vcs
categories:
  - programming
tags:
  - development
  - software
  - git
  - tfs
  - svn
  - programming
---

When you start learning to program, you rarely think about how you will be able to work on a project at the same time with other people. By the time your side projects take on larger dimensions or you work in a team, you will be confronted with the topic of version control.

Version control is basically the management of source code, so that several developers can work on the same files at the same time. The sense of a system for monitoring changes can be easily understood by an example: 

Two developers, Bob and John, are working together on a project. They do not use version control and their workflow is as follows:

The source code is stored on a network drive to which both have access. Every morning they copy the contents of the drive to their local machines. They discuss who is working on which files on that day. Bob decides to work on the connection to the database, while John decides to revise parts of the business logic. At the end of the day, both copy their changed files to the network drive. Since both have worked on different files, this is possible without any problems.

John now remembers that he also made a small change to the user interface. Knowing that Bob was only working on the database, he simply copies the file to the server. However, Bob also made some changes to this file and he also assumes that John was only updating code in the business logic. He also copies his changes and thus overwrites John's work.

When working in this way, precise communication is essential. As the example shows, this procedure is very error-prone, even though only two developers were involved. The coordination effort increases to an immeasurable extent as the number of developers increases.

I don't intend to present a multitude of version control systems, but I would like to give you at least a short overview of what has changed as time passed by. I therefore divide the different systems into three generations: Generation 0 works with file locks, Generation 1 consists of CVCS, Centralized Version Control Systems and Generation 2 are the so-called Distributed Version Control Systems (DVCS). 

The first software for version control of files was Source Code Control System (SCSS). The software classifies itself into generation 0 and locks files when they are processed. However, these systems have a significant disadvantage: assuming you want to work on a file that is currently being edited by someone else, you have to wait until they have finished making their changes. You could urge the respective developer to work faster, but he might be sick or on vacation. So it can take quite a while before you can access the file to make your changes. You could now create a local copy of the file and perform your changes right to the copy. As soon as the file itself can be accessed again, you perform a manual merge, i.e. you combine the two files and join the respective changes. However, this is again prone to errors and should be avoided.

The second generation describes the so-called centralized version control systems. Here you will find a server that manages the source code. To work on a file, a developer must check out this file, i.e. retrieve the latest version of it from the server. I would like to illustrate this again with an example of our already known developers: 

At the moment John wants to make changes to the login.html file. He loads the latest version from the server and starts working. After he has made his modifications, he checks them in, so the file on the server is being updated. In the meantime, Bob has also fetched this file and made some changes. If he wants to check in his modifications again, the server will inform him that the file has been changed in meanwhile. Bob must now perform a merge, which is usually supported or even automated by the software. Once all conflicts (places affected by both changes) have been resolved, the file can be checked in.

There are many different systems in this category, the best known probably being Subversion (SVN) and Microsoft's Team Foundation Server (TFS). While these systems are still used very often, a new category was formed around the middle of the 00s: The Distributed Version Control Systems.  First and foremost the best known representative, developed by Linux creator Linus Torvalds: Git. Git's popularity is certainly also related to the launch of GitHub 2008, but DVCS generally offer advantages over its centralized counterparts.

The idea of distributed version control systems is that no server is needed to manage the source code. Instead, each copy of the repository contains a full history of all changes made to each file. So, without loading anything from a server, you can reconstruct any point in time of your software. You can also work offline and share your changes with your team members as soon as you can reconnect to them. You also have the advantage that any computer that has a copy of the repository can be used as a backup source. I assume, however, that if you use a CVCS, you also have a corresponding backup strategy, which is why I do not consider this advantage really relevant.

I hope I could give you a rough overview of the different categories of version control systems. I already wrote an article about Git, if you like, you can read it directly on [my blog](https://blog.mjurtz.com/2017/09/getting-started-git/).

Basically, however, I would like to point out that the use of a version control system also has advantages, even if you work alone. Availability of file histories, branching or publishing to, for example, GitHub are just a few of them. But it's definitely worth trying if you don't use VCS yet.