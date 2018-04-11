---
id: 753
title: Using Androids Recyclerview with Kotlin
date: 2018-02-11T16:01:31+00:00
author: Marcel Jurtz
layout: post
guid: https://blog.mjurtz.com/?p=753
permalink: /2018/02/using-androids-recyclerview-with-kotlin/
categories:
  - android
tags:
  - android
  - app
  - development
  - programming
---
Traditionally, you were using a listview to display several homogeneous elements in you Android app. However, this view is now deprecated. Android now offers an alternative, which aims to increase performance by only loading those elements, that are currently rendered on-screen: the _RecyclerView_.

This article shows you how to get started with the RecyclerView. To make things a bit more interesting, we will be using Kotlin instead of Java.

You can set up an Android project for Kotlin by simply clicking the checkbox for Kotlin support in the new project wizard.

## Preceding Configuration

Before you can use the Recyclerview in your app, a little preparatory work is necessary. The first thing you&#8217;ll need to do is to add this dependency to your modules build.gradle file:

{% highlight xml %}implementation 'com.android.support:recyclerview-v7:26.1.0'
{% endhighlight %}

## Basic GUI Setup

The next thing you&#8217;ll want to do is to set up a simple xaml page to display the view. I&#8217;ll be using a coordinator layout, which allows me to add a FloatingActionButton at a later stage.

The snippets I use in this article are from my ShoppingList-App, which is basically a minimalistic app to save shopping lists. The user can add and remove items from shopping lists, multiple shopping lists can be used at the same time.

{% highlight xml %}<?xml version="1.0" encoding="utf-8"?>
<android.support.design.widget.CoordinatorLayout  xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context="com.jurtz.marcel.shoppinglist.MainActivity">

    <android.support.v7.widget.RecyclerView
        android:id="@+id/rvShoppingLists"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:layout_margin="8dp" />

</android.support.design.widget.CoordinatorLayout >{% endhighlight %}

## RecyclerView Components: Adapter & ViewHolder

While it offers the advantage of improved performance, the RecyclerView is a bit more complicated to create and requires additional components. In our _MainActivity_ class, the first thing to do now is to specify the RecyclerViews LayoutManager. This is required to define the appearance of the multiple items. I&#8217;d like to display them in a vertical list.

{% highlight kotlin %}rvShoppingLists.layoutManager = LinearLayoutManager(this){% endhighlight %}

The next thing you&#8217;ll want to do is to set the views adapter. You can think of the adapter as a bridge between view and model which defines the appearance of your the POJOs.

To implement the adapter, add a new class which inherits from _RecyclerView.Adapter<T>_. As you can see, this class is generic and requires a ViewHolder, I&#8217;ve also created a custom class for this purpose. The following snippet shows the methods that need to be overridden.

{% highlight kotlin %}class ShoppingListAdapter(var shoppingLists: List<ShoppingList?>) : RecyclerView.Adapter<ShoppingListViewHolder>() {

    override fun getItemCount(): Int {
        return 5
    }

    override fun onBindViewHolder(holder: ShoppingListViewHolder?, position: Int) {

    }

    override fun onCreateViewHolder(parent: ViewGroup?, viewType: Int): ShoppingListViewHolder {
        
    }
}

class ShoppingListViewHolder(val view: View) : RecyclerView.ViewHolder(view) {
   

}{% endhighlight %}

Note that I added _5_ as return value for _getItemCount()_, you can leave this for now, we will see what this does in the next steps.

## Custom Layout for RecyclerView Items

Now, we will add a new layout file (xml) to specify how each row in the RecyclerView should look like. For this purpose, I&#8217;d like to use a CardView with a nested LinearLayout that contains two TextViews. In my app, I&#8217;m using the first TextView to display a shopping lists description and the amount of items in the second one.

{% highlight xml %}<?xml version="1.0" encoding="utf-8"?>
<android.support.v7.widget.CardView xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:card_view="http://schemas.android.com/apk/res-auto"
    android:id="@+id/cvRowEntry"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    card_view:cardCornerRadius="4dp"
    android:layout_margin="8dp">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical">

        <TextView
            android:id="@+id/lblShoppingListRowItemHeader"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_margin="8dp"
            android:text="@string/rv_shopping_lists_placeholder_title"
            android:textSize="24sp" />

        <TextView
            android:id="@+id/lblShoppingListRowItemSubheader"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_margin="8dp"
            android:text="@string/rv_shopping_lists_placeholder_subtitle" />
    </LinearLayout>
</android.support.v7.widget.CardView>{% endhighlight %}

Now we want to link the view to the RecyclerView in the adapter. To do this, we will update the onCreateViewHolder method we overrode earlier.

{% highlight kotlin %}override fun onCreateViewHolder(parent: ViewGroup?, viewType: Int): ShoppingListViewHolder {
    val layoutInflater = LayoutInflater.from(parent?.context)
    val cellForRow = layoutInflater.inflate(R.layout.shopping_list_row, parent, false)
    return ShoppingListViewHolder(cellForRow)
}{% endhighlight %}

This tells the RecyclerView that each row should look like the layout we just created. If you have left the _5_ in the getItemCount()-method, you will see that the view is displayed 5 times when running the app.

## Adding real Data

Of course, that&#8217;s not all we want. Currently, we can display multiple instances of the basic view, but we want to be able to show actual data. This is where the ViewHolder comes in. Usually, you will have access to a collection of objects that will be displayed in the RecyclerView.

In my snippet above, you can see that I already added a _List<ShoppingList?>_ parameter to the adapters constructor. The ShoppingList class basically just contains a description  and a List of ShoppingListItem-objects, that again only have a _description_ field.

The first thing to do now is to update the _getItemCount()_-method to actually return the amount of available objects:

{% highlight kotlin %}override fun getItemCount(): Int {
    return shoppingLists.size ?: 0
}{% endhighlight %}

In the next step, we will link the object to the view, which is done by the following:

{% highlight kotlin %}holder?.bindShoppingList(shoppingList = shoppingLists.get(position)){% endhighlight %}

And, last but not least, declaring the views properties to the objects variable values:

{% highlight kotlin %}class ShoppingListViewHolder(val view: View) : RecyclerView.ViewHolder(view) {
    fun bindShoppingList(shoppingList: ShoppingList?) {
        view.lblShoppingListRowItemHeader.setText(shoppingList?.description)

        var suffix = ""
        var count = shoppingList?.items?.size ?: 0
        if(count == 1) {
            suffix = view.context.resources.getString(R.string.rv_shopping_list_items_suffix_single)
        } else {
            suffix = view.context.resources.getString(R.string.rv_shopping_list_items_suffix_multiple)
        }

        view.lblShoppingListRowItemSubheader.setText(count.toString() + " " + suffix)
    }
}{% endhighlight %}

That&#8217;s it! I added the following code to my MainActivity to test the adapter:

{% highlight kotlin %}rvShoppingLists.layoutManager = LinearLayoutManager(this)

var list1 = ShoppingList()
list1.description = "Groceries"

var list2 = ShoppingList()
list2.description = "Clothes"

var list3 = ShoppingList()
list3.description = "Stuff for the apartment"

var list = listOf(list1, list2, list3)

rvShoppingLists.adapter = ShoppingListAdapter(list){% endhighlight %}

&nbsp;