---
title: 'Domain Redirection with .htaccess and PHP'
date: 2020-11-07T06:00:00+00:00
author: Marcel Jurtz
layout: post
permalink: /2020/11/redirection-with-htaccess-and-php
categories:
  - webserver
---

I've been starting a new blog on my domain, for which I wanted to use the 'blog' subdomain. However, this subdomain was already in use for all my coding-related stuff. So, I decided to move my current blog from /blog to /coding and upload my new blog to /blog. So far, so good. Now I have both of my blogs up and running. There's just one problem: Any incoming traffic to existing posts on my blog would resolve to 404s. In todays post, I'm gonna show you how I resolved that issue.

First of all, my domain runs on an apache server, so I decided to capture all 404s and redirect them to a specific errorpage. To do this, I needed to add the following statement to my .htaccess file, which is located in the root directory of my webspace.

```bash
ErrorDocument 404 /notfound.php
```

Note that it's important to use a relative path to the php file. If you use an absolute path, you will override the query-string variable in php, which you might need later for the redirect. I added the notfound.php file to my blog-directory, from where it will be resolved correctly. The file looks like this:

```php
<?php
    header("Location: https://coding.mjurtz.com" . $_SERVER["REQUEST_URI"], true, 301);
    die();
?>
```

All it does is it takes the request uri, and appends it to the new domain, passing a 301 - permanent redirect back to the browser. And that's it already - My old links are still working and I can work on both blogs separately.