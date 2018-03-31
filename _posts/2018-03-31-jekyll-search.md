---
title: 'Searching Blogposts in Jekyll'
date: 2018-03-31T12:00:00+00:00
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

When clicking the search-button, the user will be redirected to the *search.html* file. This site lists all matches and a search box containing the entered values.

{% highlight html %}
---
layout: default
---
<div class="home">

<div class="site-header-container {% if site.cover %}has-cover{% endif %}" {% if site.cover %}style="background-image: url({{ site.cover | prepend: site.baseurl }});"{% endif %}>
  <div class="scrim {% if site.cover %}has-cover{% endif %}">
    <header class="site-header">
      <h1 class="title">{{ site.title }}</h1>
      {% if site.subtitle %}<p class="subtitle">{{ site.subtitle }}</p>{% endif %}
    </header>
  </div>
</div>

<div class="wrapper">    

<form action="/search.html" method="get">
  <label for="search-box">Search</label>
  <input type="text" id="search-box" name="query">
  <input type="submit" value="search">
</form>

<ul id="search-results"></ul>

</div>

<script>
  window.store = {
    {% for post in site.posts %}
      "{{ post.url | slugify }}": {
        "title": "{{ post.title | xml_escape }}",
        "author": "{{ post.author | xml_escape }}",
        "category": "{{ post.category | xml_escape }}",
        "content": {{ post.content | strip_html | strip_newlines | jsonify }},
        "url": "{{ post.url | xml_escape }}"
      }
      {% unless forloop.last %},{% endunless %}
    {% endfor %}
  };
</script>
<script src="/js/lunr.min.js"></script>
<script src="/js/search.js"></script>
{% endhighlight %}

Besides the search functionality, the page simply consists of my header and footer, the part inside the *<script>*-Tag is all that is needed to build the search functionality.