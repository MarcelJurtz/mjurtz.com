---
id: 378
title: How to set up ssh for git
date: 2017-09-11T08:04:02+00:00
author: Marcel Jurtz
layout: post
guid: http://blog.mjurtz.com/?p=378
permalink: /2017/09/set-ssh-git/
categories:
  - Tools
tags:
  - Git
  - Tools
---
When using github, you usually have to enter your login credentials each time you push. You could solve this problem by saving the credentials or by setting up ssh for your account. In this post, the process of adding and using ssh keys will be explained.

I&#8217;m using git bash, if you use git gui/sourcetree/etc. you should consider switching, at least for this process.

At first, SSH keys are generated. The command for this is the following:

<pre class="EnlighterJSRAW" data-enlighter-language="shell">ssh-keygen -t rsa -b 4096 -C "your_email@example.com"</pre>

The flag _t_ is used to describe the type, which is rsa. The next flag, _b_, describes the amount of bytes which is 4096. The last flag, C, describes the label of the key pair. Just set this email to the one you use to push to github.

After pressing enter, you are prompted to add a location to where the files should be saved. Just use the defaults here by pressing _enter_. After that, you can enter a passphrase. This will encrypt your key. I recommend doing that, if you&#8217;d like to read more about why to do that, you can go on [here](https://www.ssh.com/ssh/passphrase).

After a short time, you have two files in an .ssh-folder inside your home directory. The public key is marked by the _.pub_-ending. Enter the following commands to add the key to your ssh-agent, so you won&#8217;t have to enter the passphrase each time the keys are used:

<pre class="EnlighterJSRAW" data-enlighter-language="shell">eval $(ssh-agent -s)
ssh-add /path/to/private-key</pre>

This is all you need to do on your computer. You now need to add the public key to your github account. Log in on <https://github.com> and go to _settings_ &#8211; _SSH and GPG keys_ &#8211; _New SSH key_. Enter a title to be able to recognize the key later. After that, enter the content of your public keys file. You can log this to your terminal and copy and paste it using

<pre class="EnlighterJSRAW" data-enlighter-language="shell">clip /path/to/public-key</pre>

On Mac, you can use _pbcopy_ instead of _clip_. On Linux, just use cat or similar. (I think you basically know your stuff then.) Save the key to complete the process.

That&#8217;s basically it, your keys are now linked and you can push to github. Note that github uses different urls for https and ssh, looking like this:

<pre class="EnlighterJSRAW" data-enlighter-language="shell"># HTTPS
https://github.com/username/repo.git

# SSH
git@github.com:username/repo.git</pre>

You can check your url using _git remote -v_ and eventually change this by using _git remote set-url origin url._ (Use your specific url here).