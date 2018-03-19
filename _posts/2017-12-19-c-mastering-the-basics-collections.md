---
id: 637
title: 'C# – Mastering the Basics – Collections'
date: 2017-12-19T09:30:04+00:00
author: Marcel Jurtz
layout: post
guid: https://blog.mjurtz.com/?p=637
permalink: /2017/12/c-mastering-the-basics-collections/
categories:
  - CSharp
tags:
  - .NET
  - .NET Core
  - CSharp
  - Development
  - Programming
---
Altough the topic _&#8220;Collections&#8221;_ matches programming in general, I&#8217;ve decided to add it to my series on mastering the basics of C#. For this reason, you will find some content here that is generally applicable, but I will also discuss different language-specific elements.

Collections support several different use cases, e. g. searching through a set for objects with certain properties, or iterating through multiple elements by predefined sorting specifications.

In the first part of this article, I will cover the two basic types of collections in C#, lists and dictionaries. Then, I will go over the groups of collections you might know from Java: lists, sets and maps. I&#8217;d like to describe and compare their characteristics and then implement their functionality in C#.

## Collections in C#

The simplest form of collections in C# are arrays. An array allows you to store a group of objects of one specific type. The following example shows a string array, which contains three elements.

{% highlight c# %}String[] movies = new String[3];

movies[0] = "The Matrix";
movies[1] = "The Dark Knight";
movies[2] = "#9";

foreach(String movie in movies) {
    Console.WriteLine(movie);
}{% endhighlight %}

In this simple example, you can see the problem with arrays: the size of it needs to be known before adding the elements. The &#8220;real&#8221; collections in C# can be utilized to handle this problem.

The basis of C# collections are List<T> and Dictionary<K,T>, which will be covered in the following section.

### List<T>

Lists offer a way to organize objects in a similar way arrays do. Basically, you create a List of a specific type, which allows you to dynamically add and remove items from this list. Opposed to arrays, lists don&#8217;t need to specify the amount of objects they can contain, which allows you to dynamically change their size.

The following snippet instaniates two new instances of the class &#8220;Person&#8221;, adds both to a list and outputs the value of Name-properties (the class consists only of this one property).

{% highlight c# %}List<Person> persons = new List<Person>();

Person alice = new Person { Name = "Alice" };
Person bob = new Person { Name = "Bob" };

persons.Add(alice);
persons.Add(bob);

foreach(Person person in persons) {
    Console.WriteLine(person.Name);
}{% endhighlight %}

Besides adding items, the List<T> class offers multiple useful properties and methods, such as Count to receive the amount of objects, Clear() to remove all items or Contains() to check if the list contains a specific object. Check out the [MSDN page](https://msdn.microsoft.com/en-us/library/6sh2ey19(v=vs.110).aspx) for full documentation.

One last thing I&#8217;d like to mention is the option to access a specific item by its index, as you&#8217;re probably used to from using arrays.

### Dictionary<K,T>

The second main type of collections C# offers is the Dictionary<K,T>. As the name suggests, it allows you to keep track of multiple objects by a unique key. Both object and key can be of whatever type you&#8217;d want it to be.

The following snippet shows you how to create a dictionary with a string as key and a person as object. I&#8217;ve also added an int property named _Age_ to the class _Person_.

{% highlight c# %}Dictionary<String,Person> persons = new Dictionary<String,Person>();

Person alice = new Person { Name = "Alice", Age = 30 };
Person bob = new Person { Name = "Bob", Age = 32 };

persons.Add("alice", alice);
persons.Add("bob", bob);

Console.WriteLine($"{persons["bob"].Name} is {persons["bob"].Age} years old");
// Output: Bob is 32 years old{% endhighlight %}

As you can see, access to a dictionary works like accessing an array by replacing the index with the key. However, you can still iterate over the contained items. At this point, you can choose wether you want to iterate over the dictionary items (key & value) or just key or value.

{% highlight c# %}// Iterate over dictionary entries
foreach(var entry in persons) {
    Console.WriteLine($"Key: {entry.Key}, Name: {entry.Value.Name}, Age: {entry.Value.Age}");
}

// Iterate over dictionary values
foreach(Person person in persons.Values) {
    Console.WriteLine(person.Name);
}

// Iterate over dictionary keys
foreach(String key in persons.Keys) {
    Console.WriteLine(key);
}{% endhighlight %}

## Set

In mathematics, a set is a collection of objects, which has two main criteria: there is no given order and each element of a set is unique.

Of course, C# offers an implementation to cover such collections as well: [HashSet<T>](https://msdn.microsoft.com/en-us/library/bb359438.aspx) is your way to go. HashSets offer high performance set operations on collections that can&#8217;t hold duplicate elements nor use a specific order.

The following snippet instantiates a new HashSet of type Person and adds alice and bob to it. While you can add an object multiple times, it will be added to the set only once, which is why the output of the snippet will only display bob only once.

{% highlight c# %}HashSet<Person> persons = new HashSet<Person>();

persons.Add(bob);
persons.Add(alice);
persons.Add(bob);

foreach(Person person in persons) {
    Console.WriteLine(person.Name);
}{% endhighlight %}

## List

Compared to sets, lists are very simple to describe. Lists contain a dynamic amount of objects, whereby an object can be contained multiple times. Also, the order of items in a list is important. The previously described List<T> class already offers predefined methods to sort lists.

Besides the List<T> class, there are other variants of lists available, that serve different purposes. ArrayList<T> stores data like an array does, and List<T> basically is a wrapper for ArrayList<T>. It allows you to access the list by index and is optimized for random access.

Another option is LinkedList<T>, which maintains pointers to the actual objects along with references to the next and previous item. This results in more work when adding or removing items from the list since each objects reference must be updated. Also, you can&#8217;t access a LinkedList via index. However, if you have heavy workload in the form of tail- or head-inserts, this might be your choice.

## Map

The term map is used in Java for collections, which are known as Dictionary under C#. I&#8217;ve already covered this type of collection in the first chapter of the article, but the conclusions will be wrapped up here.

Dictionaries (or Maps) are used to associate objects to a unique key, by which they can be accessed later. One possible use case for dictionaries could be the association of user session ids with the users properties.

To conclude this, you may choose to use a dictionary when you need to look up a value by a key. If you have an object like <code class="EnlighterJSRAW" data-enlighter-language="csharp">{ key, value }</code> and you need to access both, key and value without knowing them, you might prefer list.