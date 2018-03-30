---
title: 'Sideproject - Sist!'
date: 2018-03-28T18:00:00+00:00
author: Marcel Jurtz
layout: post
permalink: /2018/03/sideproject-sist
categories:
  - Android
tags:
  - Android
  - App
  - Development
  - Programming
  - Sideproject
---

It's time to announce a new sideproject-launch!

Some days ago, I've been looking for an app which allowed my to keep track of shopping lists in a simple and fast way. While there are lots of such apps available for Android, all I tested seemed to be overkill for what I really wanted. So I decided to create my own app for this purpose.

Besides creating a simple app, my goal was to learn something new while developing the app. I decided to use Androids built in SQLite database to store lists. I've been using the database for previous projects, but I did not really care about the implementation. That being said, I wrote the queries all by hand which ended up being pretty messy.

In my defense I would like to say that I was still at the beginning of my programming career and had no idea of the existence of things like object relational mappers.

So I took a look around and decided to try Google's Room for Android. If you're not familiar with this project, feel free to checkout my article on the topic: [Introduction to Room for Android](/2018/02/introduction-to-room-for-android/).

The result of one weekend now meets my expectations: A simple Android app that fulfills the functionality I want. Multi-list management with individual entries and a focus on simplicity and speed. A small detail is for example the automatic expansion of the keyboard when adding new entries.

In addition to testing Room, I was able to try other Android features I hadn't used before. These include controls such as CardView or RecyclerView.

I would be very happy if you would test the app and submit feedback to me. As always, the entire source code is also available on GitHub and I'm always available for criticism and suggestions for improvement.

* [Download from Google Play](https://play.google.com/store/apps/details?id=com.jurtz.marcel.shoppinglist)
* [Sourcecode on GitHub](https://github.com/MarcelJurtz/ShoppingList)