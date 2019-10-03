---
id: 579
title: 'ServiceStack - Using the C#-Client'
date: 2017-11-27T18:35:13+00:00
author: Marcel Jurtz
layout: post
guid: https://blog.mjurtz.com/?p=579
permalink: /2017/11/servicestack-using-the-c-client/
categories:
  - csharp
tags:
  - dotnet
  - csharp
  - development
  - programming
  - servicestack
---
After we covered the basics of ServiceStack in part one and how to set up a service in part two of this series. Today we&#8217;ll go through how the C#-Client can be utilized to make communication between applications even easier. The sourcecode for todays article is available [GitHub](https://github.com/MarcelJurtz/BlogPosts_ServiceStack/tree/master/Part_3_CSharp_Client). If you missed one of the previous parts, feel free to check out [Part 1 &#8211; What is ServiceStack and why should I use it?](https://blog.mjurtz.com/2017/11/what-is-servicestack/) or [Part 2 &#8211; Building a Simple Service](https://blog.mjurtz.com/2017/11/servicestack-building-simple-service/).

## Implementation of the Client

First, we will need to add a new project to our solution. This will function as the client-side application. For the sake of simplicity, I decided to create a console application. After the new project has been created, we have to install the ServiceStack nuget-package. Another required step is to add a reference to our base project. This allows gives us access to the request and response DTOs.

After the project has been set up, the C#-Client can easily be utilized. The following snippet is all that it takes to send a request to the service and save its response. Don&#8217;t forget to update the clients URL to match the address of your service.

{% highlight c# %}JsonServiceClient jsonServiceClient = new JsonServiceClient("http://localhost:61401");
var response = jsonServiceClient.Post<ExpenseResponse>(new Expense { Amount = 500 });{% endhighlight %}

In order to make the example a little more valuable, we will improve the service from the last part a little bit.

## Service Improvements: Using Sessions

As you can see in the following snippet, I have installed a session mechanism that manages the current requests. I specified a general total of 1000, whereby each request reduces this amount by the requested withdrawals. Also, each request increments the request counter. I limit the amount of withdrawals to the total amount available. If more money is withdrawn, the service will not execute the request and the user will be informed. To keep track of the session data, I created another class called _TrackingData_, which is shown below the Post-method of our service.

{% highlight c# %}
public object Post(Expense request)
{
    var Session = base.SessionBag;

    var trackingData = (TrackingData)Session["Expenses"];
    if (trackingData == null)
        trackingData = new TrackingData { TotalBalance = 1000, WithdrawalsAmount = 0 };

    if(trackingData.TotalBalance >= request.Amount)
    {
        trackingData.Withdrawals += request.Amount;
        trackingData.TotalBalance -= request.Amount;
        trackingData.WithdrawalsAmount++;

        Session["Expenses"] = trackingData;

        return new ExpenseResponse
        {
            Amount = request.Amount,
            Total = trackingData.TotalBalance,
            WithdrawalsAmount = trackingData.WithdrawalsAmount,
            Status = "OK"
        };
    }
    else
    {
        return new ExpenseResponse
        {
            Amount = request.Amount,
            Total = trackingData.TotalBalance,
            WithdrawalsAmount = trackingData.WithdrawalsAmount,
            Status = "Balance too low"
        };
    }
}
{% endhighlight %}

Let&#8217;s add some further modification to be able to increase our withdrawals. These modification reads a users input as long as he enters valid integers. When entering anything else, the loop will exit and the program stops.

{% highlight c# %}
public class TrackingData
{
    public double Withdrawals { get; set; }
    public double TotalBalance { get; set; }
    public int WithdrawalsAmount { get; set;
}
{% endhighlight %}

With these adjustments it is possible to generate successive requests. After several requests, postman delivers a result similar to the following.

{% highlight json %}
{
    "Amount": 200,
    "Total": 600,
    "Status": "OK",
    "WithdrawalsAmount": 3
}
{% endhighlight %}

## Further Shortcuts

Another small change I would like to make concerns the DTOs themselves. By specifying a return value using the IReturn interface, you do not need to specify the response type on the client side.

{% highlight c# %}[Route("/Expense")]
[Route("/Expense/{Amount}")]
public class Expense : IReturn<ExpenseResponse>
{
    public double Amount { get; set;
}
    
public class ExpenseResponse
{
    public double Amount { get; set; }
    public double Total { get; set; }
    public String Status { get; set; }
    public int WithdrawalsAmount { get; set; }
}{% endhighlight %}

This change allows the usage of the client by following notation:

{% highlight c# %}var response = jsonServiceClient.Post(new Expense { Amount = 500 });{% endhighlight %}

## Final Updates for the Client

The last change I want to make is to loop the user input, so that we can test the session mechanism in our console application. The following snippet waits for user input, as long as this input is a valid integer. If not, it breaks and the program terminates.

{% highlight c# %}JsonServiceClient jsonServiceClient = new JsonServiceClient("http://localhost:61401");
int userInput;
while(Int32.TryParse(Console.ReadLine(), out userInput)) {
    var response = jsonServiceClient.Post(new Expense { Amount = userInput });
    Console.WriteLine(String.Format("Status: {0} - Withdrawals: {1} (Total: {2}, {3} Withdrawals)", response.Status, response.Amount, response.Total, response.WithdrawalsAmount));
}{% endhighlight %}

The program does not contain any logic yet, so you can mess with it in different places. However, this basic example shows how easy it is to set up the C#-Client for ServiceStack and how to use it. In the next part, we will cover the basics of authentication and authorization, which allows us to increase our services security.