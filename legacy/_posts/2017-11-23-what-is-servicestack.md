---
id: 544
title: What is ServiceStack and why should I use it?
date: 2017-11-23T20:00:40+00:00
author: Marcel Jurtz
layout: post
guid: https://blog.mjurtz.com/?p=544
permalink: /2017/11/what-is-servicestack/
categories:
  - csharp
tags:
  - dotnet
  - asp-dotnet
  - csharp
  - development
  - programming
  - rest
  - servicestack
  - xamarin
---
This article is the beginning of a series of articles in which I want to discuss the basics of building web services and designing REST APIs using ServiceStack. This first article discusses the question of what ServiceStack is all about, the benefits of it, and why you should use (or at least try out) this framework. The following articles will then discuss the design of a simple service by using a simple example to illustrate the various possibilities it offers. Each chapter ends with a summary to provide a quick way to look up previous sections.

If you can&#8217;t wait to get started, you might checkout [this](https://blog.mjurtz.com/2017/09/creating-restful-services-servicestack/) or [this](https://blog.mjurtz.com/2017/09/restful-services-servicestack-part-2/) article, which I published earlier.

## What is ServiceStack?

ServiceStack is basically an [open source](https://github.com/ServiceStack) framework for creating web services based on Microsoft&#8217;s. NET framework. The first thing I would like to mention is the excellent documentation. You&#8217;ll find all the information you might need on the [project&#8217;s website](https://servicestack.net). This also applies to the [feature overview](https://servicestack.net/features), which I&#8217;m summarizing with this article.

ServiceStack offers an alternative to the design of web services with WCF, MVC & Web API. It  may be less configurable than these, but offers improved performance on the other hand.

Another decisive argument the frameworks principle to streamline applications. The API itself is almost invisible later on, the focus lays on the POCOs. Accordingly, the creation of Web services with ServiceStack is quite simple and does not require any immense training effort. This simplicity is evident, for example, in the free choice of available formats. You can switch between displaying objects in XML and JSON (or others) without using your own logic.

Finally, ServiceStack runs under. NET and Mono, so it can be deployed on a Linux server. IIS is not mandatory. The underlying database is also selectable, supported according to the [current documentation](https://servicestack.net/ormlite) are MSSQL, MySQL, PostgreSQL and SQLite.

## Why should I use it?

As the name suggests, ServiceStack is not a specific project, but consists of different building blocks. This stack consists of serializers, service endpoints, an IoC container, ORM and caching. This means that web services do not have to integrate further libraries and can be implemented completely with servicestack. The modules are also independent of each other and can therefore be exchanged or used individually.

For example, the fast serialization mechanism can be used as a standalone tool. On the other hand, corresponding other elements can be integrated into ServiceStack, for example, the Entity Framework can be used instead of the integrated object relational mapper, _ORMLite_.

This independence of the components allows a very detailed adaptation to your own specific needs. The following three components should explain this principle a bit more clearly.

### ServiceStack.Text

This element provides the serialization functionality, which is described very well on the projects [GitHub page](https://github.com/ServiceStack/ServiceStack.Text). The library allows fast serialization and does not contain any other dependencies. Among other things, it consists of serializers for JSON, JSV and CSV and the associated auto mappers.

### ServiceStack.ORMLite

ORMLite also has a well-documented [page at GitHub](https://github.com/ServiceStack/ServiceStack.OrmLite), which describes the project as a _set of light-weight C# extension methods_ to be able to _map a POCO class 1:1 to an RDBMS table, cleanly by conventions_. Simplicity and lightness are the focus here too.

### ServiceStack.Redis

The last element I would like to mention here is [ServiceStack.Redis](https://github.com/ServiceStack/ServiceStack.Redis), a C# client for the NoSQL database (Key-Value-Store) redis. Even if the actual ServiceStack features are not used, this project allows a comfortable access mechanism to the database.

## Summary

It is difficult to put a precise definition of ServiceStack into words, so I give my best to clarify this in the following articles by demonstrating concrete examples of the way this project works. In any case, it is clear that the framework offers a fast, convenient and easy way to develop web services for communication between applications in C#. The availability of Xamarin extends this to mobile devices, which adds even more possibilites.