---
id: 372
title: Getting started with Git
date: 2017-09-10T10:45:42+00:00
author: Marcel Jurtz
layout: post
guid: http://blog.mjurtz.com/?p=372
permalink: /2017/09/getting-started-git/
categories:
  - Tools
tags:
  - Git
  - Programming
  - Tools
---
This post represents an overview of version control with git. The mentioned commands and parameters are only a small part of what&#8217;s possible. Git is a popular system for version control. Projects on git are called _Repositories_. Repositories can be used local or with a server. There are several free hosters, for example:

  * [Github](https://github.com)
  * [GitLab](https://about.gitlab.com/)
  * [BitBucket](https://bitbucket.org)

## Installation {#installation}

  * Linux (Debian-based) 
    <pre><code class="lang-Bash">apt-&lt;span class="hljs-built_in">get&lt;/span> install git-&lt;span class="hljs-built_in">all&lt;/span>
</code></pre>

  * [Windows](https://git-for-windows.github.io/)
  * [Mac](http://git-scm.com/download/mac)

## Elements {#elements}

Git-Repositories are separated into _working directory_, _staging area_ and _repository_. The working directory is the local copy of the repository.
  
Changes on the files are saved here. The repository is the shared project of all contributing persons. Using the staging area, a developert can decide, which elements he/she wants to save from the working directory to the staging area.

<img src="https://i0.wp.com/raw.githubusercontent.com/MarcelJurtz/GitTutorial/master/images/git_areas.png?w=750&#038;ssl=1" alt="Elements of a git-repository" data-recalc-dims="1" />

## Basic administration of a repository {#basic-administration-of-a-repository}

All commands are initiated with the keyword _git_.

Initialization of a folder as repository:

<pre><code class="lang-Bash">&lt;span class="hljs-comment"># New repository&lt;/span>
git init

&lt;span class="hljs-comment"># Download (clone) of an existing repository&lt;/span>
git &lt;span class="hljs-keyword">clone&lt;/span> &lt;span class="hljs-title">/path&lt;/span>/to/repository
</code></pre>

Editing / creating files: test.txt

<pre><code class="lang-Bash">&lt;span class="hljs-meta"># Overview of the current status of the repository:&lt;/span>
git status
&lt;span class="hljs-meta"># -&gt; test.txt is displayed red&lt;/span>

&lt;span class="hljs-meta"># Splitting into separate parts:&lt;/span>
&lt;span class="hljs-meta"># Untracked Files: New files&lt;/span>
&lt;span class="hljs-meta"># Changes not staged for commit: Edited files&lt;/span>

&lt;span class="hljs-meta"># Parameters:&lt;/span>
&lt;span class="hljs-meta"># -u: Add all edited files, but no new ones.&lt;/span>
&lt;span class="hljs-meta"># -A: Add all files (alternative: git add . )&lt;/span>


&lt;span class="hljs-meta"># Adding files to the index&lt;/span>
git add test.txt
&lt;span class="hljs-meta"># -&gt; test.txt ist now displayed green&lt;/span>

&lt;span class="hljs-meta"># Adding files to the repository&lt;/span>
git commit -m &lt;span class="hljs-string">"Add test"&lt;/span>
</code></pre>

Each commit is commented with the -m-flag.
  
This gives the developers a fast overview about the changes made.

Git has some [naming-conventions](https://github.com/erlang/otp/wiki/Writing-good-commit-messages), that tells you to write a commit

  * short (<50 characters)
  * written in present tense
  * expressive

<pre><code class="lang-Bash"># &lt;span class="hljs-built_in">Print&lt;/span> &lt;span class="hljs-built_in">all&lt;/span> commits including details &lt;span class="hljs-keyword">in&lt;/span> chronological order
git &lt;span class="hljs-built_in">log&lt;/span>
</code></pre>

The output of this command looks like the following:

<img src="https://i0.wp.com/raw.githubusercontent.com/MarcelJurtz/GitTutorial/master/images/git_log.png?w=750&#038;ssl=1" alt="Git log output" data-recalc-dims="1" />

  * SHA1-checksum
  * Name of the author
  * Email of the author

## Configuring names and emails {#configuring-names-and-emails}

This configuration is global for every repository (set it up once per device).

<pre><code class="lang-Bash">git config --global user&lt;span class="hljs-selector-class">.name&lt;/span> &lt;span class="hljs-string">"Marcel Jurtz"&lt;/span>
git config --global user&lt;span class="hljs-selector-class">.email&lt;/span> jurtzmarcel@gmail.com
</code></pre>

You can set this for each project individually, just leave out the _&#8211;global_-flag.

## HEAD {#head}

In the following, _HEAD_ is often mentioned. HEAD is the most recent commit of the current branch.

## Using Remotes {#verwendung-von-remotes}

A remote is a repository on a server / different pc, for example hosted on Github.

<img src="https://i0.wp.com/raw.githubusercontent.com/MarcelJurtz/GitTutorial/master/images/git_areas_extended.png?w=750&#038;ssl=1" alt="Remotes on git-repositories" data-recalc-dims="1" />

In the following, I use Github as remote server.

After creating an account on [Github](https://www.github.com), available _repositories_ are shown and new ones can be created.

When creating a new repository, you can select to add a README- and/or license- & .gitignore-file.

Es wird zu Beginn die Möglichkeit gegeben, eine README-, sowie eine license- und eine .gitignore-Datei anzulegen:

  * README is a file that represents general information about the project. When hosted on GitHub, this file can be imagined as _landing page_ for visitors. README-files are usually written in markdown. Markdown is similar to HTML, but has lesser functionality and better readability (in my opinion).
  * license: All repositories on Github (free accounts) are publicly available. With a license-file, the usage of the repository can be adjusted to different preferences. GitHub offers some pre-defined licenses, for example MIT and GPLv3, along with information on the differences.
  * .gitignore: Gitignore-files list files and directories, that will be ignored by git. You can add files and folders, to, for example, prevent temp- or meta-files from being added to the repository.

The following screenshot shows an exemplary repository on Github.

<img src="https://i2.wp.com/raw.githubusercontent.com/MarcelJurtz/GitTutorial/master/images/git_repo.png?w=750&#038;ssl=1" alt="Git repository" data-recalc-dims="1" />

The lading page offers some overview of a repository, for example the files and folders in the root-directory, amount of commits, branches, licensing-info and so on.

After a repository has been created, it can be added as a remote:

<pre><code class="lang-Bash">git remote &lt;span class="hljs-keyword">add&lt;/span>&lt;span class="bash"> origin url&lt;/span>
</code></pre>

You can set individual names for the remote, I use _origin_ here.

To upload changes commited in the working directory, use the following:

<pre><code class="lang-Bash">git &lt;span class="hljs-built_in">push&lt;/span> &lt;span class="hljs-built_in">origin&lt;/span> master
</code></pre>

And this to download changes from a remote:

<pre><code class="lang-Bash">git pull origin &lt;span class="hljs-literal">master&lt;/span>
</code></pre>

Please know, that if you have an existing project on your pc and want to add it to a remote, which already contains files, for example README, license, or .gitignore,
  
you&#8217;ll have to merge the repositories. If you don&#8217;t have any of these files locally, you&#8217;ll have no problems, since the files will be automatically implemented.
  
If you do, this will result into a merge-conflict. Continue reading, to find out more about merging.

## Merging {#merging}

For example, user A and user B are editing the same repository, both users can work with the above mentioned methods.
  
A problem comes up, when both users push the same file: merge conflicts.

Think of a file in the remote repository named test.txt, which has the following content:

    <span class="hljs-attribute">Hello</span>
    

Using pull, both users get this file to their working directories.
  
Now, both users edit the file to the following:

Version A:

    Hello, I am <span class="hljs-keyword">A</span>!
    

Version B:

    Hello, I am B!
    

Both users now push their version to the remote. The user submitting first will have no problems doing so. In my example, this will be user B.
  
When user A pushes his updates, the push will result in an error like the following:

    CONFLICT (content): <span class="hljs-keyword">Merge</span> conflict <span class="hljs-keyword">in</span> test.txt
    <span class="hljs-keyword">Automatic</span> <span class="hljs-keyword">merge</span> <span class="hljs-keyword">failed</span>; fix conflicts and then <span class="hljs-keyword">commit</span> the result.
    

Git now modified the mentioned files, test.txt now has the following content:

    <<<<<<< HEAD:test.txt
    <span class="hljs-section">Hello, I am B!
    =======</span>
    Hello, I am A!
    >>>>>>> commit-text:test.txt
    

A now has to decide, which version is preferred and then commit the changes.
  
A updates the file to the following:

    Hello, we are A <span class="hljs-keyword">and</span> B!
    

After that, he creates a new commit to document the merge:

<pre><code class="lang-Bash">git &lt;span class="hljs-keyword">add&lt;/span>&lt;span class="bash"> test.txt
&lt;/span>git commit -m &lt;span class="hljs-string">"Resolve Merge Conflict"&lt;/span>
</code></pre>

## Branching {#branching}

Git uses _branches_ to support developing new features. This supports developing new functionality, without the need to edited the working current version of the project.

<img src="https://i1.wp.com/raw.githubusercontent.com/MarcelJurtz/GitTutorial/master/images/branching.png?w=750&#038;ssl=1" alt="Git Branching" data-recalc-dims="1" />

Use this to create a new branch:

<pre><code class="lang-Bash">&lt;span class="hljs-symbol">git&lt;/span> &lt;span class="hljs-keyword">branch &lt;/span>&lt;span class="hljs-keyword">branch_name&lt;/span>
</code></pre>

And this to view all existing branches:

<pre><code class="lang-Bash">&lt;span class="hljs-attribute">git branch&lt;/span>
</code></pre>

The current branch is marked by an asterisk.
  
Usually, this is _master_ in new projects.
  
As you may notice, we used this branch earlier to push commits.

Replace this to push to other branches.

Changing branches

<pre><code class="lang-Bash">&lt;span class="hljs-attribute">git&lt;/span> checkout branch_name
</code></pre>

Creating a branch and switching to it:

<pre><code class="lang-Bash">git checkout -&lt;span class="hljs-selector-tag">b&lt;/span> branch_name
</code></pre>

When a branch is changed, files in the working directory are automatically updated to match that branch.
  
This way, you don&#8217;t have to use different folders for different branches and save space on your harddrive.

Die beiden Befehle können auch zusammengefasst werden:

To merge two branches, switch to the branch you want to merge to (using `git checkout`) and merge them:

<pre><code class="lang-Bash">git &lt;span class="hljs-keyword">merge&lt;/span> branch_name
</code></pre>

To view differences in branches, you can use

<pre><code class="lang-Bash">git &lt;span class="hljs-built_in">diff&lt;/span> source_branch destination_branch
</code></pre>

Branches, that are no longer needed, can be deleted:

<pre><code class="lang-Bash">&lt;span class="hljs-symbol">git&lt;/span> &lt;span class="hljs-keyword">branch &lt;/span>-d &lt;span class="hljs-keyword">branch_name&lt;/span>
</code></pre>

## Tags {#tags}

Tags can be added to commits, to highlight completed versions of a project. To select a commit, the first 10 characters of the checksum are needed (`git log`).

<pre><code class="lang-Bash">&lt;span class="hljs-comment"># Creating a new tag&lt;/span>
git &lt;span class="hljs-keyword">tag&lt;/span> &lt;span class="hljs-title">1&lt;/span>.&lt;span class="hljs-number">0.0&lt;/span> d525ac9ddf

&lt;span class="hljs-comment"># Show all tags&lt;/span>
git &lt;span class="hljs-keyword">tag&lt;/span>

&lt;span class="hljs-title"># Creating&lt;/span> a commented &lt;span class="hljs-keyword">tag&lt;/span>
&lt;span class="hljs-title">git&lt;/span> &lt;span class="hljs-keyword">tag&lt;/span> &lt;span class="hljs-title">1&lt;/span>.&lt;span class="hljs-number">0.0&lt;/span> d525ac9ddf -m &lt;span class="hljs-string">"Commented Tag"&lt;/span>

&lt;span class="hljs-comment"># Show commit using its tag&lt;/span>
git show &lt;span class="hljs-number">1.0&lt;/span>.&lt;span class="hljs-number">0&lt;/span>
</code></pre>

## Resetting changes {#resetting-changes}

To reset contents of the working directory, you can use the following:

<pre><code class="lang-Bash">git &lt;span class="hljs-keyword">reset&lt;/span> &lt;span class="hljs-comment">-- filename&lt;/span>
</code></pre>

This command obtains changes from HEAD and overwrites the content of the working directory to match HEAD.
  
Changes, that have been added to the staging area are not overwritten!

To reset contents of the directory, use this:

<pre><code class="lang-Bash">git &lt;span class="hljs-keyword">reset&lt;/span> &lt;span class="hljs-comment">--hard d525ac9&lt;/span>
</code></pre>

As shown earlier, you need to add the first characters of the commits checksum to identify the commit.
  
This command should only be used with caution, since it changes the commit-history.

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

If you don&#8217;t like using the terminal, there are some GUI-clients. For example:

  * [SourceTree](https://www.sourcetreeapp.com/)

## Links {#links}

  * [Git Reference](https://git-scm.com/docs)
  * [Git Cheat Sheet](https://services.github.com/on-demand/downloads/github-git-cheat-sheet.pdf)
  * [Markdown Tutorial](http://www.markdowntutorial.com)