---
id: 513
title: Reusable Styles for Android
date: 2017-11-17T18:00:27+00:00
author: Marcel Jurtz
layout: post
guid: http://blog.mjurtz.com/?p=513
permalink: /2017/11/reusable-styles-android/
categories:
  - Android
tags:
  - Android
  - Clean Code
  - Development
---
View definitions under android are often used for many different controls. However, there is an easy way to recycle defined styles and use them for other controls, often saving a lot of code. So, styles basically allow you to move your repeatedly used styles out of the layout file into a styles.xml file (which is located in the values-folder). This article covers the basics of using styles on Android.

Think of the following layout, which represents two simple buttons inside a LinearLayout:

{% highlight xml %}<LinearLayout
        android:orientation="horizontal"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_weight="0">

        <Button
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:text="@string/cmdPrevPage"
            android:id="@+id/cmdPrevPage" />

        <Button
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:text="@string/cmdNextPage"
            android:id="@+id/cmdNextPage" />

</LinearLayout>{% endhighlight %}

The buttons look nearly identical, which leads us to creating a new entry in styles.xml looking like this:

{% highlight xml %}<style name="button">
        <item name="android:layout_width">match_parent</item>
        <item name="android:layout_height">wrap_content</item>
        <item name="android:weight">1</item>
</style>{% endhighlight %}

The style contains all the elements, that both buttons have in common. Of course, this does not include its ID and text properties. To apply this to the buttons, simply change the first snippet to the following:

{% highlight xml %}<LinearLayout
        android:orientation="horizontal"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_weight="0">

        <Button
            style="@style/button"
            android:text="@string/cmdPrevPage"
            android:id="@+id/cmdPrevPage" />

        <Button
            style="@style/button"
            android:text="@string/cmdNextPage"
            android:id="@+id/cmdNextPage" />

</LinearLayout>{% endhighlight %}

This small example already shows the advantage of using styles: your layout becomes clearer and if you want to change some layouts, you only have to do this in one place.

However, this approach is not the end of the story. Often only small details of controls are different, for example the positioning of a button on the left or right. In order to be able to handle such minimal changes in case of multiple use, we can use inheritance. Let us go back to the previous example to demonstrate this principle.

One of the buttons is used to mark &#8220;Next&#8221;, the other one for &#8220;Back&#8221;. Let&#8217;s say the code is developed for an app that represents a blog. With these two buttons you can switch back and forth between the available pages. It is realistic, however, that such a navigation is implemented for the articles themselves, but also for categories or tags, which is why a simple possibility for multiple use is required. With the following two styles this can be realized very easily:

{% highlight xml %}<style name="button_prev" parent="@android:style/button">
    <item name="android:text">@string/cmdPrevPage</item>
</style>

<style name="button_next" parent="@android:style/button">
    <item name="android:text">@string/cmdNextPage</item>
</style>{% endhighlight %}

Further information on this topic is available in the [Android developer documentation](https://developer.android.com/guide/topics/ui/look-and-feel/themes.html).