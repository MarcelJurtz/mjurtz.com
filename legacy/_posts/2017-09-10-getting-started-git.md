---
id: 372
title: Getting started with Git
date: 2017-09-10T10:45:42+00:00
author: Marcel Jurtz
layout: post
guid: http://blog.mjurtz.com/?p=372
permalink: /2017/09/getting-started-git/
categories:
  - tools
tags:
  - git
  - programming
  - tools
---
This post represents an overview of version control with git. The mentioned commands and parameters are only a small part of what's possible. Git is a popular system for version control. Projects on git are called _Repositories_. Repositories can be used local or with a server. There are several free hosters, for example:

  * [Github](https://github.com)
  * [GitLab](https://about.gitlab.com/)
  * [BitBucket](https://bitbucket.org)

## Installation {#installation}

* Linux (Debian-based)

{% highlight bash %}
apt-get install git-all
{% endhighlight %}

* [Windows](https://git-for-windows.github.io/)
* [Mac](http://git-scm.com/download/mac)

## Elements {#elements}

Git-Repositories are separated into _working directory_, _staging area_ and _repository_. The working directory is the local copy of the repository.
  
Changes on the files are saved here. The repository is the shared project of all contributing persons. Using the staging area, a developert can decide, which elements he/she wants to save from the working directory to the staging area.

![Areas of a git repository](/assets/2017/getting-started-git-areas.png)

## Basic administration of a repository {#basic-administration-of-a-repository}

All commands are initiated with the keyword _git_.

Initialization of a folder as repository:

{% highlight bash %}
# New repository
git init

# Download (clone) of an existing repository
git clone path/to/repository
{% endhighlight %}

Editing / creating files: test.txt

{% highlight bash %}
# Overview of the current status of the repository
git status
# test.txt is displayed red

# Splitting into separate parts:
# Untracked Files: New files
# Changes not staged for commit: Edited files

# Parameters:
# -u: Add all edited files, but no new ones.
# -A: Add all files (alternative: git add . )

# Adding files to the index
git add test.txt
# test.txt ist now displayed green

# Adding files to the repository
git commit -m "Add test"
{% endhighlight %}

Each commit is commented with the -m-flag.
  
This gives the developers a fast overview about the changes made.

Git has some [naming-conventions](https://github.com/erlang/otp/wiki/Writing-good-commit-messages), that tells you to write a commit

  * short (<50 characters)
  * written in present tense
  * expressive

{% highlight bash %}
# Print all commits including details in chronological order
git log
{% endhighlight %}

The output of this command looks like the following:

![Output of the git log command](/assets/2017/getting-started-git-log.png)

  * SHA1-checksum
  * Name of the author
  * Email of the author

## Configuring names and emails {#configuring-names-and-emails}

This configuration is global for every repository (set it up once per device).

{% highlight bash %}
git config --global user.name "Marcel Jurtz"
git config --global user.email jurtzmarcel@gmail.com
{% endhighlight %}

You can set this for each project individually, just leave out the _&#8211;global_-flag.

## HEAD {#head}

In the following, _HEAD_ is often mentioned. HEAD is the most recent commit of the current branch.

## Using Remotes {#verwendung-von-remotes}

A remote is a repository on a server / different pc, for example hosted on Github.

![Remotes for shared repositories](/assets/2017/getting-started-git-remote.png)

In the following, I use Github as remote server.

After creating an account on [Github](https://www.github.com), available _repositories_ are shown and new ones can be created.

When creating a new repository, you can select to add a README- and/or license- & .gitignore-file.

When creating a new repository (of course this can be done later as well), you are prompted to create a README-, license- and .gitignore-file:

  * README is a file that represents general information about the project. When hosted on GitHub, this file can be imagined as _landing page_ for visitors. README-files are usually written in markdown. Markdown is similar to HTML, but has lesser functionality and better readability (in my opinion).
  * license: All repositories on Github (free accounts) are publicly available. With a license-file, the usage of the repository can be adjusted to different preferences. GitHub offers some pre-defined licenses, for example MIT and GPLv3, along with information on the differences.
  * .gitignore: Gitignore-files list files and directories, that will be ignored by git. You can add files and folders, to, for example, prevent temp- or meta-files from being added to the repository.

The following screenshot shows an exemplary repository on Github.

![Example of a GitHub repository](/assets/2017/getting-started-git-github.png)

The lading page offers some overview of a repository, for example the files and folders in the root-directory, amount of commits, branches, licensing-info and so on.

After a repository has been created, it can be added as a remote:

{% highlight bash %}
git remote add origin url
{% endhighlight %}

You can set individual names for the remote, I use _origin_ here.

To upload changes commited in the working directory, use the following:

{% highlight bash %}
git push origin master
{% endhighlight %}

And this to download changes from a remote:

{% highlight bash %}
git pull origin master
{% endhighlight %}

Please know that if you have an existing project on your pc and want to add it to a remote, which already contains files, for example README, license, or .gitignore,
  
youll have to merge the repositories. If you don't have any of these files locally, you'll have no problems, since the files will be automatically implemented.
  
If you do, this will result into a merge-conflict. Continue reading, to find out more about merging.

## Merging {#merging}

For example, user A and user B are editing the same repository, both users can work with the above mentioned methods.
  
A problem comes up, when both users push the same file: merge conflicts.

Think of a file in the remote repository named _test.txt_, which has the following content:

> Hello    

Using pull, both users get this file to their working directories.
  
Now, both users edit the file to the following:

Version A:

> Hello, I am A!

Version B:

> Hello, I am B!

Both users now push their version to the remote. The user submitting first will have no problems doing so. In my example, this will be user B.
  
When user A pushes his updates, the push will result in an error like the following:

> CONFLICT (content): Merge conflict in test.txt  
> Automatic merge failed; fix conflicts and then commit the result.
    

Git now modified the mentioned files, test.txt now has the following content:

>  \<\<\<\<\<\<\< HEAD:test.txt  
>  Hello, I am B!  
>  =======  
>  Hello, I am A!  
>  \>\>\>\>\>\>\> commit-text:test.txt
    

A now has to decide, which version is preferred and then commit the changes.
  
A updates the file to the following:

>  Hello, we are A and B!

After that, he creates a new commit to document the merge:

{% highlight bash %}
git add test.txt
git commit -m "Resolve Merge Conflict"
{% endhighlight %}

## Branching {#branching}

Git uses _branches_ to support developing new features. This supports developing new functionality, without the need to edited the working current version of the project.

![Branches in git](/assets/2017/getting-started-git-branching.png)

Use this to create a new branch:

{% highlight bash %}
git branch branch_name
{% endhighlight %}

And this to view all existing branches:

{% highlight bash %}
git branch
{% endhighlight %}

The current branch is marked by an asterisk. Usually, this is _master_ in new projects. As you may notice, we used this branch earlier to push commits. Replace this to push to other branches.

Changing branches

{% highlight bash %}
git checkout branch_name
{% endhighlight %}

Creating a branch and switching to it:

{% highlight bash %}
git checkout -b branch_name
{% endhighlight %}

When a branch is changed, files in the working directory are automatically updated to match that branch. This way, you don't have to use different folders for different branches and save space on your harddrive.

To merge two branches, switch to the branch you want to merge to (using `git checkout`) and merge them:

{% highlight bash %}
git merge branch_name
{% endhighlight %}

To view differences in branches, you can use

{% highlight bash %}
git diff source_branch destination_branch
{% endhighlight %}

Branches, that are no longer needed, can be deleted:

{% highlight bash %}
git branch -d branch_name
{% endhighlight %}

## Tags {#tags}

Tags can be added to commits, to highlight completed versions of a project. To select a commit, the first 10 characters of the checksum are needed (`git log`).

{% highlight bash %}
# Creating a new tag
git tag 1.0.0 d525ac9ddf

# Show all tags
git tag

# Creating a commented tag
git tag 1.0.0 d525ac9ddf -m "Commented Tag"

# Show commit using its tag
git show 1.0.0
{% endhighlight %}

## Resetting changes {#resetting-changes}

To reset contents of the working directory, you can use the following:

{% highlight bash %}
git reset -- filename
{% endhighlight %}

This command obtains changes from HEAD and overwrites the content of the working directory to match HEAD. Changes that have been added to the staging area are not overwritten!

To reset contents of the directory, use this:

{% highlight bash %}
git reset --hard d525ac9
{% endhighlight %}

As shown earlier, you need to add the first characters of the commits checksum to identify the commit. This command should only be used with caution, since it changes the commit-history.

## Conventions & Good Practices {#conventions-good-practices}

To keep repositories as clear as possible, you should follow some general rules in naming commits and branches.

### Commits {#commits}

Commits should be written under considering the following:

  * Keep it short and simple
  * Begin with a normalized set of verbs, written in present tense, for example: _add_, _remove_, _update_

### Branches {#branches}

It is useful to use a short and precise formulation here as well. Often, a verb as prefix is used to describe the purpose of the branch. For example: _add/feature_a_, _remove/feature_b_.

When contributing to public projects, you should also check the previous commits to adjust to the organization of the other authors.

## Alternatives {#alternatives}

If you don't like using the terminal, there are some GUI-clients. For example:

  * [SourceTree](https://www.sourcetreeapp.com/)

## Links {#links}

  * [Git Reference](https://git-scm.com/docs)
  * [Git Cheat Sheet](https://services.github.com/on-demand/downloads/github-git-cheat-sheet.pdf)
  * [Markdown Tutorial](http://www.markdowntutorial.com)