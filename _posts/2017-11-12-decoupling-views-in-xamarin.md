---
id: 473
title: Decoupling Views in Xamarin
date: 2017-11-12T10:05:27+00:00
author: Marcel Jurtz
layout: post
guid: http://blog.mjurtz.com/?p=473
permalink: /2017/11/decoupling-views-in-xamarin/
categories:
  - CSharp
tags:
  - .NET
  - CSharp
  - Development
  - Programming
  - Windows
  - Xamarin
---
As a software developer, you&#8217;ve probably stumbled across old source code you&#8217;ve written from time to time and you&#8217;ve just been thinking &#8216;how the hell can someone come up with such bullshit?&#8217;. I don&#8217;t see myself as an exception regarding this topic and just recently had such a case, which I would like to document in this article. More specifically, I want to discuss the decoupling of views under xamarin. The source I use in this article is available on my [GitHub profile](https://github.com/MarcelJurtz/XamarinDecouplingDemo).

## Xamarin Navigation

Decoupling allows you to use multiple views without dependencies between them. Without decoupling, all relevant elements would have to be edited for individual changes. Xamarin relies on a simple page-based navigation structure that is managed in the form of a stack (see figure). A new page is added with Push(), the topmost page can be removed with Pop().

<img class="aligncenter size-medium wp-image-476" src="https://i1.wp.com/blog.mjurtz.com/wp-content/uploads/2017/11/XamarinNavigation-500x347.png?resize=500%2C347" alt="Xamarin Navigation Scheme" height="347" width="500" srcset="https://i1.wp.com/blog.mjurtz.com/wp-content/uploads/2017/11/XamarinNavigation.png?resize=500%2C347&ssl=1 500w, https://i1.wp.com/blog.mjurtz.com/wp-content/uploads/2017/11/XamarinNavigation.png?w=1466&ssl=1 1466w" sizes="(max-width: 500px) 100vw, 500px" data-recalc-dims="1" />

These pages are usually interdependent, for example, information is required on a page that is entered on the next page. After entering the required information, the user is taken back to the calling page and the information is transferred to this one. This is exactly the point I want to clarify with this article. The strict interaction of the sides is problematic in the case of subsequent changes.

## Building the demo application

Take a look at the following screenshots to see how the demo application works. The app consists of two pages, whereby the second view is opened by a button on the first one. The second view contains an input field whose value will then be accessed in the first view. There is an additional button to abort the process. If the &#8220;Confirm&#8221; button is pressed even though no text has been entered, the user will be informed.

<img class="aligncenter size-full wp-image-479" src="https://i0.wp.com/blog.mjurtz.com/wp-content/uploads/2017/11/Xamarin_Decoupling_Screenshots.png?resize=750%2C266" alt="Xamarin Decoupling Screenshots" height="266" width="750" srcset="https://i0.wp.com/blog.mjurtz.com/wp-content/uploads/2017/11/Xamarin_Decoupling_Screenshots.png?w=1200&ssl=1 1200w, https://i0.wp.com/blog.mjurtz.com/wp-content/uploads/2017/11/Xamarin_Decoupling_Screenshots.png?resize=500%2C177&ssl=1 500w" sizes="(max-width: 750px) 100vw, 750px" data-recalc-dims="1" />

The default configuration of Visual Studio already creates a default page for every new Xamarin project. I created an additional one next to it. For interaction I have created an interface which manages the possible handlers. The folder structure of my main project consists of the following files:

  * App.xaml (+ code-behind)
  * MainPage.xaml (+ code-behind)
  * InputPage.xaml (+code-behind)
  * IRequireDialogInteraction

Since this project is used as showcase to demonstrate the decoupling of views, I don&#8217;t follow the MVVM-approach here. However, this is done only to minimize possible distractions, MVVM can of course still be used.

The first thing you&#8217;ll want to do is to set up a navigation page. This is done in App.xaml.cs by adding <code class="EnlighterJSRAW" data-enlighter-language="csharp">MainPage = new NavigationPage(new MainPage());</code>. After doing so, you can navigate between your views via <code class="EnlighterJSRAW" data-enlighter-language="csharp">Navigation.PushAsync(page);</code>.

The next thing is setting up the simple GUI:

MainPage.xaml

<pre class="EnlighterJSRAW" data-enlighter-language="xml">&lt;?xml version="1.0" encoding="utf-8" ?&gt;
&lt;ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamarinDecouplingDemo"
             x:Class="XamarinDecouplingDemo.MainPage"&gt;
    &lt;ContentPage.Content&gt;
        &lt;StackLayout&gt;
            &lt;Button
                Text="Load second page"
                Clicked="OnLoadButtonClick"/&gt;
            &lt;Label
                x:Name="lblInputText" /&gt;
        &lt;/StackLayout&gt;
    &lt;/ContentPage.Content&gt;
&lt;/ContentPage&gt;</pre>

InputPage.xaml

<pre class="EnlighterJSRAW" data-enlighter-language="xml">&lt;?xml version="1.0" encoding="utf-8" ?&gt;
&lt;ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamarinDecouplingDemo.InputPage"&gt;
    &lt;ContentPage.Content&gt;
        &lt;Grid
                Padding="30"
                BackgroundColor="White"&gt;
            &lt;Grid.RowDefinitions&gt;
                &lt;RowDefinition Height="Auto" /&gt;
                &lt;RowDefinition Height="Auto" /&gt;
                &lt;RowDefinition Height="Auto" /&gt;
            &lt;/Grid.RowDefinitions&gt;
            &lt;Grid.ColumnDefinitions&gt;
                &lt;ColumnDefinition Width="*" /&gt;
                &lt;ColumnDefinition Width="*" /&gt;
            &lt;/Grid.ColumnDefinitions&gt;

            &lt;Label
                    Grid.ColumnSpan="2"
                    Grid.Row="0"
                    HorizontalOptions="Start"
                    Text="Text to return"/&gt;

            &lt;Entry
                    Grid.ColumnSpan="2"
                    Grid.Row="1"
                    x:Name="txtTextToReturn"/&gt;

            &lt;Button
                    Grid.Column="0"
                    Grid.Row="2"
                    x:Name="cmdCancel"
                    Text="Cancel"
                    Clicked="OnCancel"/&gt;

            &lt;Button 
                    Grid.Column="1"
                    Grid.Row="2"
                    x:Name="cmdConfirm"
                    Text="Confirm"
                    Clicked="OnConfirm" /&gt;

        &lt;/Grid&gt;
    &lt;/ContentPage.Content&gt;
&lt;/ContentPage&gt;</pre>

The second view can be left by clicking one of two buttons, cancel and confirm. These interactions will be required by the MainPage later, which is why methods for those will be added to _IRequireDialogInteraction_:

<pre class="EnlighterJSRAW" data-enlighter-language="csharp">interface IRequireDialogInteraction
{
    void OnDialogConfirmation(object sender, EventArgs e);
    void OnDialogCancellation(object sender, EventArgs e);
}
</pre>

After the basic settings have been made, the actual magic will now happen in form of public EventHandler properties. MainPage needs a property of the type _InputPage_, which is used to interact with the second view. _InputPage_ on the other hand will implement two event handlers, which can be accessed by _MainPage_. These will then be linked to the _Clicked_-property which is set in the xaml-file:

<pre class="EnlighterJSRAW" data-enlighter-language="csharp">public partial class InputPage : ContentPage
{
    public InputPage ()
    {
      InitializeComponent();
    }

    public EventHandler Confirmed;
    public EventHandler Cancelled;

    public String enteredText { get { return txtTextToReturn.Text; } }

    public void OnConfirm(object sender, EventArgs e)
    {
        Confirmed?.Invoke(this, EventArgs.Empty);
    }

    public void OnCancel(object sender, EventArgs e)
    {
        Cancelled?.Invoke(this, EventArgs.Empty);
    }
}</pre>

_MainPage_ now only needs to link the event handlers to the methods which are implemented by _IRequireDialogInteraction_ and can then access the _enteredText_-property of the _InputPage_-object:

<pre class="EnlighterJSRAW" data-enlighter-language="csharp">public partial class MainPage : ContentPage, IRequireDialogInteraction
{
    private InputPage _InputPage;

    public MainPage()
    {
        InitializeComponent();
    }

    public void OnDialogCancellation(object sender, EventArgs e)
    {
        Navigation.PopAsync();
    }

    public void OnDialogConfirmation(object sender, EventArgs e)
    {
        Navigation.PopAsync();
        lblInputText.Text = _InputPage.enteredText ?? "No text entered.";
    }

    public void OnLoadButtonClick(object sender, EventArgs e)
    {
        lblInputText.Text = "";

        _InputPage = new InputPage();
        _InputPage.Confirmed += OnDialogConfirmation;
        _InputPage.Cancelled += OnDialogCancellation;

        Navigation.PushAsync(_InputPage);
    }
}</pre>

And that&#8217;s basically it! If you want to change the style in which user interaction happens, you&#8217;ll be able to simply swap out the specific part in the methods _OnDialogConfirmation_ or _OnDialogCancellation_.

## Implementing Dialogs

Finally, I would like cover one specific detail which helped me a lot in my work with xamarin. [This plugin](https://github.com/rotorgames/Rg.Plugins.Popup) (available on GitHub & Nuget) allows you to show regular XAML-pages as dialogs. This is really helpful in combination with the decoupling shown above, since you can create custom dialogs, for example to display user prompts or general input fields to take off the load of some of the main pages. The usage looks exactly the same as that of the regular pages, except that the dialog-pages must inherit from _PopupPage_ and are called with _PushPopupAsync()_ instead of _PushAsync()_. The closing of the page takes place analogously with _PopPopupAsync()_.