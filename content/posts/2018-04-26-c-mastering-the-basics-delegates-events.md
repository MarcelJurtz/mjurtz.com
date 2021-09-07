---
title: C# - Mastering the Basics - Events and Delegates
date: 2018-04-26T10:00:00+00:00
author: Marcel Jurtz
layout: post
permalink: /2018/04/c-mastering-the-basics-events-delegates
categories:
  - csharp
tags:
  - dotnet
  - csharp
  - development
  - programming
---

Delegates and Events are powerful tools in C#, but they can definitely be confusing in the beginning. Todays article covers the basics of this topic and helps you to get comfortable implementing this approach in your own applications.

## Delegates

I think, the main problem is that delegates are often explained unnecessarily complicated. For this reason, I want to give you an easily understandable introduction. Let's look at a basic delegate declaration:

```csharp 
private delegate void myDelegate(string a, string b);
```

The declaration of a delegate looks very similar to a method declaration. In this case, the delegate looks like a method without a return value, but with two parameters, each of type string. The only difference is the keyword ‘delegate’ and the missing method body. Imagine delegates not as methods, but as references to methods.

Let's look at the following two methods:

```csharp 
private static void SayHello(string yourName, string myName)
{
  Console.WriteLine($"Hello {yourName}, I am {myName}");
}

private static void AppendStrings(string a, string b)
{
 	Console.WriteLine(a + b);
}
```

It is not difficult to understand what these two methods do. Note, however, the signature of both methods: it is identical to that of the previously defined delegate. This means that the delegate can be used as a reference for the methods. The only decisive factor here is the type of the respective parameter, not the name.

```csharp 
myDelegate del1 = new myDelegate(SayHello);
myDelegate del2 = new myDelegate(AppendStrings);

del1("Internet", "Marcel");
del2("Some", "Text");
```

To create this reference, a new instance of the delegate is created. The compiler creates a fully-fledged class from the delegate, which is why instantiation can be done as usual using the *new* keyword. As an argument, the delegate receives the name of the desired method. The referenced method can now be called via the instantiated delegate.

### Multicast Delegates

A strong feature of delegates, the so-called multicasting, enables the chaining of method calls. Let us illustrate this with an example:

```csharp 
myDelegate del1 = new myDelegate(SayHello);
myDelegate del2 = new myDelegate(AppendStrings);

del1 += del2;
del1("Hello", "World");
```

This will execute the stored methods for *del1* and *del2* using the arguments "Hello" and "World" for both methods. That is based on the so-called invocation list. Each delegate has such a list which holds references to all specified methods, which then are called when invoking the delegate.

```csharp 
Delegate[] invocationList = del1.GetInvocationList();
```

You can access a delegate's invocation list using the *GetInvocationList* method. In this example, you will see that *del1* has two entries here. The entries in the InvocationList are always made in the stored sequence.

To conclude this chapter, I'd like to show you a way to change arguments as you work through the invocation list. Delegates can also have return values, but this is not helpful during the processing steps. When using chained delegates with return types, the result of the last item from the invocation list will be returned.

The key is to use reference types instead of value types. Doing so, a reference, not a copy of the variable, is passed to the method. To achieve this, the parameters must be marked with the ref keyword. I adjusted the content of my *SayHello()* method to demonstrate the whole thing. After the text is printed to the console, the method removes all vowels from the *yourName* variable.

```csharp 
private delegate void myDelegate(ref string a, ref string b);

static void Main(string[] args)
{
  myDelegate del1 = new myDelegate(SayHello);
  myDelegate del2 = new myDelegate(AppendStrings);

  string hello = "Hello";
  string world = "World!";
        
  del1 += del2;
  del1(ref hello, ref world);
}

private static void SayHello(ref string yourName, ref string myName)
{
  Console.WriteLine($"Hello {yourName}, I am {myName}");
  yourName = new string(yourName.Where(c => !"aeiou".Contains(c)).ToArray());
}

private static void AppendStrings(ref string a, ref string b)
{
  Console.WriteLine(a + b);
}
```

## Events

Events indicate the occurrence of an action. Imagine an event as a notification to all interested parties. The trick here is that the raiser of the event does not have to know about the parties that are interested in that (subscibers). When an event occurs, data can optionally be sent. In C# this is done using the EventArgs class or a derivation of that class.

Events basically just encapsulate delegates. This is probably the point that took me the longest to embrace. I'm going to illustrate the whole thing with an example. With our current knowledge we can build the following application:

```csharp 
public partial class Form1 : Form
{
  public Form1()
  {
    InitializeComponent();

    Worker w = new Worker();
    w.WorkCompleted += new WorkCompletedHandler(OnWorkerWorkCompleted);
    w.Work();
  }

  private void OnWorkerWorkCompleted(int hoursWorked)
  {
    MessageBox.Show($"Worker has been completed: {hoursWorked} hours worked.");
  }
}

public delegate void WorkCompletedHandler(int hoursWorked);

class Worker
{
  public WorkCompletedHandler WorkCompleted;

  private void OnWorkCompleted(int hoursWorked)
  {
    Console.WriteLine("Work Completed - Inside Worker");
    WorkCompleted?.Invoke(hoursWorked);
  }

  public void Work()
  {
    for(int i = 0; i < 3; i++)
    {
      System.Threading.Thread.Sleep(1000);
      Console.WriteLine("Working...");
    }
    OnWorkCompleted(3);
  }
}
```

I think you'll quickly grasp what's happening here, as well. The application launches and instantiates a worker object. The work method is called, which waits three time for one second and then outputs some information. Finally, the Delegate WorkCompletedHandler is called, to which the worked time is passed. The application now displays a message box and notifies the user that the work method has been completed. The only thing to remember here is the linkage of the OnWorkerCompleted method to the delegate's invocation list.

Let’s take a look at how that looks when using an event:

```csharp 
public partial class Form1 : Form
{
  public Form1()
  {
    InitializeComponent();

    w = new Worker();
    w.WorkCompleted += new WorkCompletedHandler(OnWorkerWorkCompleted);
    w.Work();
  }

  private void OnWorkerWorkCompleted(int hoursWorked)
  {
    MessageBox.Show($"Worker has been completed: {hoursWorked} hours worked.");
  }
}

public delegate void WorkCompletedHandler(int hoursWorked);

class Worker
{
  public event WorkCompletedHandler WorkCompleted;

  private void OnWorkCompleted(int hoursWorked)
  {
    Console.WriteLine("Work Completed - Inside Worker");
    WorkCompleted?.Invoke(hoursWorked);
  }

  public void Work()
  {
    for(int i = 0; i < 3; i++)
    {
      System.Threading.Thread.Sleep(1000);
      Console.WriteLine("Working...");
    }
    OnWorkCompleted(3);
  }
}
```

Looks almost the same, right? As I said before, events are just wrappers for delegates. So theoretically you can use both versions I have presented. However, events are used for the same reason as properties are used: It is considered bad practice to expose fields directly. According to the principle of encapsulation, these are thus isolated from the outside. That is exactly the case here as well. Events are used to encapsulate access to delegates.

The final thing I want to share with you in this article is the use of custom EventArgs. The.NET framework has a convention whereby a delegate type used for an event always takes two parameters. The first one is a reference to the object that triggers the event. The other one is an object of the class EventArgs or one that inherits from it. We want to create our own EventArgs inheritance that contains the hours worked. The following code does that for us:

```csharp 
public partial class Form1 : Form
{
  public Form1()
  {
    InitializeComponent();

    Worker w = new Worker();
    w.WorkCompleted += new WorkCompletedHandler(OnWorkerWorkCompleted);
    w.Work();
  }

  private void OnWorkerWorkCompleted(object sender, WorkerEventArgs e)
  {
    MessageBox.Show($"Worker has been completed: {e.HoursWorked} hours worked.");
  }
}

public delegate void WorkCompletedHandler(object sender, WorkerEventArgs e);

class Worker
{
  public event WorkCompletedHandler WorkCompleted;

  private void OnWorkCompleted(int hoursWorked)
  {
    Console.WriteLine("Work Completed - Inside Worker");
    WorkCompleted?.Invoke(this, new WorkerEventArgs(3));
  }

  public void Work()
  {
    for(int i = 0; i < 3; i++)
    {
      System.Threading.Thread.Sleep(1000);
      Console.WriteLine("Working...");
    }
    OnWorkCompleted(3);
  }
}

public class WorkerEventArgs : EventArgs
{
  public WorkerEventArgs(int hoursWorked)
  {
    _hoursWorked = hoursWorked;
  }

  private int _hoursWorked;
  public int HoursWorked { get { return _hoursWorked; } }
}

```

## Wrap-Up

Delegates are references to methods. The compiler creates a class when a delete is generated, instances of the delegate are then created as usual with the new keyword. Like methods, delegates can have a return type. Utiliting multicasting, delegates can be chained together, the call order is based on the delegates invocation list. Events are used to encapsulate delegates. Events usually use two parameters, the first being a reference to the trigger and the second an instance of EventArgs or an inheriting class thereof.
