---
title: 'Garbage Collection in .NET'
date: 2018-09-16T06:00:00+00:00
author: Marcel Jurtz
layout: post
permalink: /2018/09/dotnet-garbage-collection
categories:
  - development
tags:
  - development
  - software
  - programming
  - csharp
  - dotnet
---

Today I would like to give you an overview of how the garbage collector of the .NET framework works. Knowledge of it makes a lot of sense to improve your programming skills regarding object creation and lifetime.

# Garbage Collection in General

I want to start with some general theory - what is garbage collection and what do I need it for? Garbage collection (GC) is basically a feature that relieves developers of the work of allocating and deallocating memory for their objects. 

In low-level programming languages, you as the developer have to take care of this allocation and deallocation. If you want the computer to remember an object, you allocate memory for that object. When the object is no longer needed, you have to free up the memory. 

Although this concept leads to very efficient programs, it is also error-prone. For example, if your program doesn't free memory, but asks for more and more, this can lead to a memory leak, which in turn can lead to the consumption of your computer's entire memory. Alternatively, the program simply no longer responds to user interaction. Another source of errors are bugs that cause your program to attempt to access a memory section that has not yet been allocated. In this case, the operating system will terminate your program immediately. 

There are more fragile issues, but they are too many to list. The concept of higher-level programming languages in this respect is identical: when you need an object, you request an instance of the respective class. This instance is then used in your program, and as long as there is a reference to the instance, memory is reserved for it. As soon as nothing in your code refers to the instance anymore, the corresponding memory will be freed again.

Basically, you can memorize that programming languages with GC are usually less efficient than those without. This is due to the fact that the cleanup process is usually performed at regular intervals. For this reason, there is a potential for wasted memory between operations. For data-intensive applications, there is also the possibility that many (de)allocations may occur, which is why the program looks as if it is not responding.

# Garbage Collection in .NET

In general, garbage collection in .NET works as described above. However, I'd like to cover the topic in a bit more detail.

## Memory Allocation in .NET

In the .NET framework, each process has a virtual address space. As an application developer, you never access physical memory, but always an abstraction level of it. The garbage collector takes care of the allocation and deallocation of virtual memory on the managed heap. 

Blocks of the virtual memory are always in one of three states: 

* Free: There are no references to this block, it is ready for referencing. 
* Reserved: The memory block is free, but reserved for a request and cannot be used by other applications. Until the memory area is assured, no data can be stored. 
* Assured: The memory block is directly assigned to a physical memory. 

Gaps can occur in virtual memory. When memory is requested, a suitable space is searched in which the memory is completely available. For example, if you need 100 MB of memory in the working memory, the search will continue until a single free block of at least 100 MB memory is found. Smaller blocks will be skipped.

You may be familiar with the principle of swap files, especially when you are at home in the Linux world. If available memory becomes too small, it can be swapped out to disk, so called swap files are created. .NET uses this concept, but can also swap out under certain conditions, even if the memory is not full. 

### Triggering the Garbage Collection 

There are several different ways in which exactly the garbage collection can be triggered. Firstly, of course, if the system has little physical memory. This is usually recognized by a message from the operating system. Another possibility is to exceed a limit value of the memory used by objects in the heap. This limit depends on the program and is adjusted at runtime. 

A third method is to manually call the [GC.Collect() method](https://docs.microsoft.com/en-gb/dotnet/api/system.gc.collect?view=netframework-4.7.2). However, you will hardly need this case (except perhaps for testing the functionality of the garbage collector).

### Managed Heap

I have already mentioned the managed heap several times, but have not explained it further. The Managed Heap describes a memory area of the physical memory, which was reserved by the garbage collector after its instantiation by the CLR for the administration of objects. The operating system manages a system heap in parallel.

The reservation and release of memory segments is done by the Win32 methods [VirtualAlloc()](https://msdn.microsoft.com/library/aa366887.aspx) and [VirtualFree()](https://msdn.microsoft.com/library/aa366892.aspx).

The more objects there are on the heap, the more complex is the garbage collection process. When releasing memory, active objects are pushed together. This reduces the required memory space, but the objects retain their locality (jointly instantiated objects lie next to each other). 

The size of the memory usage also determines the frequency and size of the individual GC operations. 

### Generations 

The heap is built in generations to describe the lifetime of objects. Garbage collection usually takes the form of releasing short-lived objects that occupy only a small part of the working memory. A distinction is made between three generations: 

Generation 0 is the most recent generation. It contains short-lived objects such as local variables. 

Generation 1 also contains short-lived objects, but serves as a buffer between short-lived and long-lived objects. 

Generation 2 finally contains the long-lived objects. These are, for example, objects with static data that are retained throughout the entire application runtime. 

A garbage collection always takes place for a particular generation, if the appropriate conditions are met. The collection within a generation always contains the younger generations. A collection in generation 2 is therefore also referred to as a complete collection because it cleanses all objects in the managed heap. 

Objects that still exist after a garbage collection are moved up the generation hierarchy. If the garbage collector recognizes that this number is very high in a generation, the threshold value for memory allocations for this generation is increased. It is always weighed between two priorities: The size of the memory used by an application and the amount of garbage collection required. 

GC more often takes place in younger generations. If more working memory is required, the older generations (including those below them) are cleaned. 

### Garbage collection procedure 

At the start of a GC, a list of active objects is created. The objects to be compressed (active objects moved together) are then reassigned. Storage space for inactive objects is released. Objects are now promoted in their generation. 

## GC in practice 

You have probably already worked with unmanaged resources. In this case, the IDisposable interface can be used to share manually used memory of unmanaged resources. This is usually seen in the form of file access. The construction of a corresponding code snippet is usually based on the construction of a using block. After finishing the code of this block, the Dispose() method is called, which releases the resources again. IDisposable in itself has nothing directly to do with Garbage Collection. However, manual release is required when objects access unmanaged resources, as in the following example: If managed objects refer to unmanaged objects, these must first be released manually so that the managed objects can be cleaned up. 

The following example creates and releases a StreamReader instance: 

```csharp  
using (StreamReader s = new StreamReader("file1.txt"))) { 
    // … 
} 
```

The using block does not have to be used, alternatively Dispose() can be called within a finally block. Dispose itself does not belong to the garbage collection, so it is not called in a collection! This construct can be used for any class that implements IDisposable.

You should also not rely on the garbage collector. It's good style to close database connections or the like by yourself, instead of leaving this task to the GC.

Also, you should have little reason to initiate the GC yourself. Exceptions occur when you have reason to believe that there is a large number of objects in generations 1 and 2 that are ready to be cleaned up. This may be the case, for example, if you have just closed a large form whose controls are all no longer needed. 

# Conclusion 

I hope that this article has given you a rough insight into how the Garbage Collection works in C#. But the topic itself gives enough theory for a whole book, so I recommend to the inclined reader to read more detailed articles. [Microsoft's own documentation](https://docs.microsoft.com/en-gb/dotnet/standard/garbage-collection/fundamentals) on the subject is certainly a good starting point, have fun with it! 
