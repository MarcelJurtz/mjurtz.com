---
title: C# - Mastering the Basics - The Repository Pattern
date: 2018-05-13T10:00:00+00:00
author: Marcel Jurtz
layout: post
permalink: /2018/05/c-mastering-the-basics-repository-pattern
categories:
  - csharp
tags:
  - dotnet
  - csharp
  - development
  - programming
---

There’s a very large number of articles about the repository pattern out there.
Nevertheless, most of them overlap in the description and cause confusion among developers.
For this reason I decided to write my own article to document my opinion.

In this article I will first clarify the what and why of the repository pattern.
Afterwards I will explain how the pattern can look like in C#.

In his book, Patterns of Enterprise Application Architecture, Martin Fowler describes the Repository Pattern as a “Mediate between the domain and data mapping layers, acting like an in-memory collection of domain objects”.

The following benefits of utilizing this pattern are usually listed when reading articles on the topic:

* Minimize query logic
* Decoupling from persistence frameworks
* Easier testing

The idea of reducing query logic refers to the reusability of code. Imagine you work directly on a collection. If you need a certain subset of the stored objects, you will query them via LINQ. But chances are, you won't need this query logic in just one place. However, since the entire collection is available everywhere, you will implement this logic in each place, resulting in duplicated code. The alternative is to move this logic to the repository. All code elements involved then retrieve the subset from this.

The decoupling from persistence frameworks follows the same principle. The connection to data storage only takes place in the repository. The business logic only accesses the repository, the implementation details are irrelevant at this point. This means that the repository can be exchanged at any time, for example with another ORM or even to a new storage strategy.

The idea behind the simplification of testability is to make repositories easily exchangeable by using an interface and thus to be able to easily implement a mock repository. But this is not a peculiarity of the repository pattern, so I don't really see this point as an advantage of the pattern.

## In a Nutshell

As mentioned above, the basic idea of the repository pattern is to design an intermediate layer so that the business logic can operate as if on an in-memory collection. In a very simple way, this interface can look like this:

* Add(obj)
* Remove(obj)
* Get(id)
* GetAll()
* Find(predicate)

As you can see, there is no item for saving changes. And that's exactly where the in-memory collection comes into play. Since the collections only contain object references, the entries can be edited directly.

Nevertheless, database entries must somehow be persisted. This is where the Unit-of-Work Pattern comes in. Again, from Martin Fowler: A unit of work maintains a list of objects affected by a business transaction and coordinates the writing out of changes.

So this means that we will combine one or more repositories with a single unit of work which will persist changes to the database. An interface for a basic unit of work could have the following methods and properties:

* IMyRepository MyRepository { get; }
* SaveChanges();

Of course, in a real application, there would be some more repositories.

If you look at the whole thing now, you might get the idea that Entity Framework already implements these mechanisms. More precisely, a DbSet looks like a repository and a DbContext like a unit of work. Let's take a closer look:

At a surface level, a DbSet looks like a repository. It provides access to a collection and allows you to edit, add and remove entries. Using the DbContext you can persist changes afterwards. However, this approach is problematic in view of the previously defined advantages of the repository pattern: One point was the reduction of duplicated source code by outsourcing functionalities with regard to special subsets. With this approach here the pure collection is published and worked with. The result is exactly what we want to avoid, namely the direct selection and projection of items from the collection.

The use of DbContext is also not suitable here. Another positive argument of the repository pattern was the decoupling of persistence frameworks. This contradicts the approach, since the business logic would be tied to Entity Framework.

So while DbSet and DbContext look like implementations of the repository and unit of work pattern, they don’t provide the architectural benefits, which is why you still have to provide your own implementation for this.
