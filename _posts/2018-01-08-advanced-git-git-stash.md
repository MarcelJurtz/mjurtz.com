---
id: 708
title: 'Advanced Git - Git Stash'
date: 2018-01-08T06:00:26+00:00
author: Marcel Jurtz
layout: post
guid: https://blog.mjurtz.com/?p=708
permalink: /2018/01/advanced-git-git-stash/
categories:
  - tools
tags:
  - development
  - git
  - tools
---
In my [introductory post on git](https://blog.mjurtz.com/2017/09/getting-started-git/), I told you that git uses three areas to track changes. Well, maybe I lied to you in this case. There are actually four areas available and I will cover this fourth area in todays post.

The fourth area is called Stash. Basically, it works like a clipboard to which you can save current changes.

## Concepts of the Stash &#8230;

Imagine the following scenario: You are working on a change. In the middle of your work you remember that you also wanted to do something else. This other change is important and should be handled immediately. However, it cannot be implemented with the uncomitted changes you are currently working on. In short, you need the status of the last commit, but you don&#8217;t want to discard your current changes and you also can&#8217;t commit them yet.

This is where the stash comes in handy. The stash basically allows you to copy the changes that are located in your working area and index and stores them. It also checks out the latest commit so your working area and your index a re in a clean state. You can have multiple of such _clipboard contents_ on your machine and you can restore each of them individually.

## &#8230; and how to apply them

All of the stash commands are initiated by _git stash_. To stash the current changes, use _git stash save_. For this, you can also simply use _git stash_ without any suffixes. If you now use _git status_ to view your changes, you&#8217;ll see that there are none available and your working area and index are in a clean state.

You can then use _git stash list_ to view all the contents of the stash. You&#8217;ll notice that each entry has an id, which can be used to restore an individual entry. Per default, the most recent entry will be used.

To restore an entry from the stash, use _git stash apply <id>_. This will restore the changes to your working area and index, but the entry will also remain in the stash. To clear all entries from the stash, use _git stash clear_.