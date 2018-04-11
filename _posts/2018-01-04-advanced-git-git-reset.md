---
id: 702
title: 'Advanced Git &#8211; Git Reset'
date: 2018-01-04T19:09:23+00:00
author: Marcel Jurtz
layout: post
guid: https://blog.mjurtz.com/?p=702
permalink: /2018/01/advanced-git-git-reset/
categories:
  - tools
tags:
  - development
  - git
  - productivity
  - tools
---
In [one of my earliest posts](https://blog.mjurtz.com/2017/09/getting-started-git/), I wrote an introduction to version control, more specifically: git. The topic I will cover today extends this article by a topic which is actually not that complicated. However, people seem to find it hard to deal with it and use it as a highway to StackOverflow.

I talk about _git reset_. This command can be used to reset changes in your working area or to remove staged files from the index. I think that one of the problems here is the context-dependent functionality of the command. Another problem is the commands potentially disruptive character, since it can lead to data loss.

## Moving the current Branch

In general, you can split the commands functionality into two separate parts. First, the command will move the current branch. Let&#8217;s look at an example. In my current project, I&#8217;ve made some changes that I&#8217;d like to reset. In my example, my current commit **25a5d0e** contains unwanted information and I&#8217;d like to return back to the state at commit **ea02e01**.

![Git Reset - Move HEAD](/assets/2018/git_reset_move_head.png)

Of course, this is a very simple example, git reset is not restricted to a single branch. I&#8217;d also like to mention, that this operation will lead to commits that don&#8217;t belong to any branch. In this case, git will automatically detect and delete them using its garbage collection mechanism.

## Copy Files

As I mentioned earlier, the reset command moves the current branch (which is actually a pointer to a specific commit). The second part of its functionality is to optionally copy contents from the repository to the index (also known as _staging area_) and the working area.

To specify where contents should be copied to, you can use the parameters <span style="text-decoration: underline;"><em>&#8211;hard</em></span>, _&#8211;mixed_ or _&#8211;soft_. First of these copies the contents from the repository to both the index, and the working area. Appending _&#8211;mixed_ will lead to the contents being copied only to the index, not to the working area. When using _git reset_ without any parameters, this is what will be applied. The last option is to use _&#8211;soft_, which will prevent any content from being copied. In this case, only the branch-movement-part of the command will be executed.

![Git Reset - Move HEAD](/assets/2018/git_reset_copy_files.png)

## Conclusion

Working with _git reset_ is easier than you think. Also, it&#8217;s definitely helpful to always think of the two components. Doing so, there is another a really cool feature that you may already have seen: You can use _git reset_ to unstage files. Alternatively to _git rm &#8211;cached_, you can use _git reset HEAD &#8211;mixed_, which will overwrite the staged files.