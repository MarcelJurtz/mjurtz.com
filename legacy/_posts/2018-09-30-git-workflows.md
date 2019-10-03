---
title: 'Git Workflows'
date: 2018-09-30T06:00:00+00:00
author: Marcel Jurtz
layout: post
permalink: /2018/09/git-workflows
categories:
  - development
tags:
  - git
  - vcs
  - organization
---

Almost all programming projects work with some kind of version control.
When I started to work with Git, I used the tool also directly for my private projects. 
But especially at the beginning I found it hard to structure my commits and branches in a practical way. 
For this reason I would like to show you some common strategies today, the so-called Git Workflows.

# Simple Workflow

The simple workflow consists of a single master branch. 
There is only this one branch to which changes are pushed. 
This workflow is only suitable for very small projects, e.g. private ones, where only you work on yourself. 
As the team grows, this workflow becomes very messy and you' re going to have to deal with a lot of merge conflicts.

<a href="/assets/2018/git_workflow_simple.png" data-lightbox="git_workflow_simple"><img src="/assets/2018/git_workflow_simple.png"/></a>

# Feature Branches

This second level adds feature branches to the simple workflow. 
These branches are used to develop new functionalities separately from the rest of the project. 
After a feature is completed, the branch is merged. 
Unlike the master branch, the feature branches are therefore short-lived and only exist until their merge. 
Depending on their complexity, feature branches can often be further subdivided. 
Just make sure you don't exaggerate, which could again affect the overall structure.

<a href="/assets/2018/git_workflow_feature.png" data-lightbox="git_workflow_feature"><img src="/assets/2018/git_workflow_feature.png"/></a>

# Developer Branch

With the Developer Branch, a second, long-lived branch is created next to the Master Branch. 
This is the only place where development takes place, so that the master branch always remains in a release-ready state. 
Here, however, similar problems arise as with the simple workflow, which is why it should only be used for very small teams.

<a href="/assets/2018/git_workflow_develop.png" data-lightbox="git_workflow_develop"><img src="/assets/2018/git_workflow_develop.png"/></a>

# Developer and Feature Branches

The previous two strategies can be combined very well. 
Again, the master branch must always be ready for release, feature branches are only ever merged with the developer branch. 
After successful testing of the functionalities on the developer branch, this branch is merged to master, which then can be released.

<a href="/assets/2018/git_workflow_dev_feature.png" data-lightbox="git_workflow_dev_feature"><img src="/assets/2018/git_workflow_dev_feature.png"/></a>

# Release Branches

This extension of the developer and feature branch workflow is often used for large projects that are planning frequent releases. 
For a new release, a new release branch is created from the developer branch. 
This only is used for final bug fixes, no new features are developed here. 
As soon as the release can be shipped, the branch will be merged into both the master and the developer branch. 
The fixes in the release branches allow other teams to work on new features without disturbing the work on the release.

The model is often complemented by another branch: the hotfix branch which allows direct bug fixing from the master branch.

<a href="/assets/2018/git_workflow_release.png" data-lightbox="git_workflow_release"><img src="/assets/2018/git_workflow_release.png"/></a>

# But which concept is right for me?

Basically, the more complex your project, the more complex the workflow should be. 
But also for one-man projects it often makes sense not to use the simple workflow and to use a branching strategy already here. 
For my own projects, for example, I currently use the Developer Branch concept.
But whatever you decide to do: Make sure you have a consistent naming strategy for branches (and commits, of course) 
and you're very good to go.
