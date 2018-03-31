---
title: 'Searching Blogposts in Jekyll'
date: 2018-03-30T12:00:00+00:00
author: Marcel Jurtz
layout: post
permalink: /2018/03/jekyll-search
categories:
  - Blog
tags:
  - Blog
  - Web
  - Ruby
  - JavaScript
  - Jekyll
---

In one of my [previous posts](/2018/03/moving-to-jekyll), I went over the details why I moved my blog from WordPress to Jekyll. So far, I really appreciate my new system, but I miss one particular feature: searching through my posts.

Since Jekyll is a static site generator and does not rely on a database for the posts, you can't simply add a search functionality like WordPress does. So, I have to work with some frontend / clientside stuff.

After doing some research, I stumbled across [lunr.js](https://lunrjs.com/), a JavaScript-based text search library to use on the client side.

I decided to give lunr a try and went on to add a search box to my main page. You can now see this in the very bottom of the screen. The is constructed as follows:

{% highlight html %}
<form action="/search.html" method="get">
  <label for="search_box">Search Posts</label>
  <input type="text" id="search_box" name="query">
  <input type="submit" value="search">
</form>
{% endhighlight %}

When clicking the search-button, the user will be redirected to the *search.html* file. This site lists all matches and a search box containing the entered values. The search page consists of my regular header and footer. Also, of course, it contains a script which fetches all matching posts. It then lists all matches inside an unordered list. Of course, all scripts and pages are also located in this Blogs [GitHub Repository](https://github.com/MarcelJurtz/blog.mjurtz.com), feel free to check it out! 