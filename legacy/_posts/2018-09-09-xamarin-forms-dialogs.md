---
title: 'Dialogs in Xamarin.Forms'
date: 2018-09-09T06:00:00+00:00
author: Marcel Jurtz
layout: post
permalink: /2018/09/xamarin-forms-dialogs
categories:
  - development
tags:
  - development
  - software
  - programming
  - xamarin
  - cross-platform
---

If you already have a little experience in cross-platform development with Xamarin.Forms, 
you probably know the problem: Navigation between different pages works very well, 
but there is no easy solution to display dialog boxes overlaying an active page.

Today I want to show you how you can implement this feature in your Xamarin.Forms application. 
I have created an example project for this purpose, which you can download from my 
[GitHub site](https://github.com/MarcelJurtz/XamarinDialogDemo).

## Setup

I first create a new Cross-Platform project in Visual Studio. 
I start without a template (blank) and use .NET standard for the shared code base. 
After the project is initialized, I add the plugin for the dialogs to all projects. 
You can find it via [Nuget](https://www.nuget.org/packages/Rg.Plugins.Popup/). 
The project is also open source on [GitHub](https://github.com/rotorgames/Rg.Plugins.Popup). 
To add a Nuget package to your projects, mark the project (or solution) and select the entry 
*Manage NuGet Packages*. Select all projects of your solution to install the package.

The first thing to do is to initialize the package. This takes place in the platform-specific projects. 
For Android, open the MainActivity.cs file and add the following to your *OnCreate()* method:

{% highlight csharp %}
protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);

    Rg.Plugins.Popup.Popup.Init(this, bundle);
        
    Xamarin.Forms.Forms.Init(this, bundle);
    LoadApplication (new App ());
}
{% endhighlight %}

The same goes for iOS: Add the following snippet to your *FinishedLaunching()*-Method inside the AppDelegate.cs file.

{% highlight csharp %}
public override bool FinishedLaunching(UIApplication app, NSDictionary options)
{
    Rg.Plugins.Popup.Popup.Init();
      
    global::Xamarin.Forms.Forms.Init ();
    LoadApplication (new App ());
    return base.FinishedLaunching (app, options);
}
{% endhighlight %}


If you want to support UWP, the relevant code goes inside the App-class:

{% highlight csharp %}
Rg.Plugins.Popup.Popup.Init();
Xamarin.Forms.Forms.Init (e);

if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
{
  // ...
}
{% endhighlight %}

More detailed information about initializing the plugin can be found in the 
[projects documentation](https://github.com/rotorgames/Rg.Plugins.Popup/wiki/Getting-started).

## Integrating Dialogs

Now add a new Content Page to the .NET standard project. This is generated with predefined code, which must be adjusted as follows:

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<pages.PopUpPage xmlns="http://xamarin.com/schemas/2014/forms" 
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:pages="clr-namespace:Rg.Plugins.Popup.Pages;assembly=Rg.Plugins.Popup"
             x:Class="PopupDemo.Dialog">
    
</pages.PopUpPage>
{% endhighlight %}

This is the foundation of the dialog. I just removed the sample content and added the namespace for the plugin. 
Finally I changed the root element of the page from ContentView to PopUpPage. 
These changes are also required in the code-behind file. 
All you have to do here is change the inheritance from ContentView to PopUpPage.

Next, I design the content of the dialog window. I want to show the user a simple login dialog, 
so I need input fields for username and password, as well as a button to confirm the input.

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<pages:PopupPage xmlns="http://xamarin.com/schemas/2014/forms" 
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:pages="clr-namespace:Rg.Plugins.Popup.Pages;assembly=Rg.Plugins.Popup"
             xmlns:animations="clr-namespace:Rg.Plugins.Popup.Animations;assembly=Rg.Plugins.Popup"
             x:Class="PopupDemo.Dialog">
    <pages:PopupPage.Animation>
        <animations:ScaleAnimation DurationIn="200"
                                   DurationOut="300"
                                   EasingIn="SinIn"
                                   EasingOut="SinInOut"
                                   HasBackgroundAnimation="True"
                                   PositionIn="Top"
                                   PositionOut="Bottom"
                                   ScaleIn="1.2"
                                   ScaleOut="0.8"/>
    </pages:PopupPage.Animation>
    
    <StackLayout
        Margin="12"
        Padding="24"
        Spacing="24"
        BackgroundColor="White"
        HorizontalOptions="Center"
        VerticalOptions="Center">

        <StackLayout>
            <Label
                Text="Username"/>
            <Entry
                FontSize="Large"
                Text="me@example.com" />
        </StackLayout>

        <StackLayout>
            <Label
                Text="Password"/>
            <Entry
                FontSize="Large"
                IsPassword="True"
                Text="1234" />
        </StackLayout>

        <StackLayout>
            <Button
                Clicked="Button_Clicked"
                BackgroundColor="Orange"
                FontSize="Large"
                Text="Submit"
                TextColor="White"/>
        </StackLayout>
    </StackLayout>
</pages:PopupPage>
{% endhighlight %}

Now there is missing only one possibility to display the dialog. 
I added a simple button to my main page and linked it to an event handler that opens the dialog. 
In real applications, you would do this by adding bindings to the ViewModel, 
but I decided to use the quick and dirty variant to keep the example as simple as possible.

Calling the dialog probably reminds you syntactically of the normal navigation between the pages. 
Closing the dialog works analogously, I wired this part with the EventHandler in the code-behind file of the dialog.

{% highlight csharp %}
// Open Dialog
PopupNavigation.Instance.PushAsync(new Dialog());

// Close Dialog
PopupNavigation.Instance.PopAsync();
{% endhighlight %}

## Animations

If you now start the application on a device or emulator of your choice, everything should already work. 
Besides the pure dialog functionality, the plugin offers the possibility to animate the dialogs. 
The animation is defined in the XAML code of the dialog. 
You can also find complete documentation for this on the 
[projects website](https://github.com/rotorgames/Rg.Plugins.Popup/wiki/Animations). 
Here it's a good idea to just play around with the available possibilities, but finally you'll find a small example:

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<pages:PopupPage xmlns="http://xamarin.com/schemas/2014/forms" 
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:pages="clr-namespace:Rg.Plugins.Popup.Pages;assembly=Rg.Plugins.Popup"
             xmlns:animations="clr-namespace:Rg.Plugins.Popup.Animations;assembly=Rg.Plugins.Popup"
             x:Class="PopupDemo.Dialog">
    <pages:PopupPage.Animation>
        <animations:ScaleAnimation DurationIn="200"
                                   DurationOut="300"
                                   EasingIn="SinIn"
                                   EasingOut="SinInOut"
                                   HasBackgroundAnimation="True"
                                   PositionIn="Top"
                                   PositionOut="Bottom"
                                   ScaleIn="1.2"
                                   ScaleOut="0.8"/>
    </pages:PopupPage.Animation>
    
    <StackLayout
	…
{% endhighlight %}

<iframe width="560" height="315" src="https://www.youtube.com/embed/xgXXz-FT2I4?rel=0" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>
