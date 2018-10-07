---
title: 'Semantic Versioning'
date: 2018-10-07T06:00:00+00:00
author: Marcel Jurtz
layout: post
permalink: /2018/10/semantic-versioning
categories:
  - development
tags:
  - versioning
  - management
  - documentation
  - release
  - organization
---

Have you ever wondered what kind of methodology you should use to label the releases of your software? 
Many projects use a structure with three numbers, each separated by a dot. 
I have good news for you: This structure is very common and well documented. 
It's called Semantic Versioning and you'll find all the details about it [here](https://semver.org/). 

# Building Version Identifiers with SemVer 

Version numbers are structured according to the X.Y.Z. structure. 
X stands for the major version, Y for the minor version and Z for the patch version.

Changes to the major version indicate incompatible changes to the API, so that software that depends on your software will not necessarily work after an upgrade. Changes to the minor version signal added backward compatible features, while changes to the patch area point out backward compatible bugfixes. 

As you can see, the setup is very simple, but it definitely makes sense. 

# Why SemVer?

As I said before: Because it makes sense. Basically, it is essential to mark the versions of software according to a predefined style. By strictly specifying when which part of the version number is increased, you don't have to think about how to label the release.  

The system also helps to make your project easier to use for other people. For example, the 1.4.42 version identifier tells you directly that this is the first major release and the fourth minor release. You can also see that 42 patches have already been applied, which suggests a not inconsiderable amount of bug fixes.

In the case of integrated libraries, it is easy to prevent a package from being upgraded that would break the application. If the major version changes, some testing is required before upgrading.

# How Do I Use The System?

A common question is how to start versioning. Contrary to what you might have thought, the first version is not marked 0.0.1, but 0.1.0. This makes sense because you usually start with a rough draft of your project, not a bugfix. 

Another frequent question is when to release the first major version. Once your application is able to be used in production, you can set the version to 1.0.0. Before that time, there are usually big changes and upheavals in the software, so you shouldn't be afraid of introducing breaking changes there. This is really only relevant after the go-live.  

# Pre-Releases 

Before you release a new major version, you probably want to publish pre-releases. This is done by appending a usually alphanumeric identifier to the normal version code, separated by a hyphen. You can use this to identify alpha or beta versions, for example.

# Conclusion

Do you or your company already use Semantic Versioning? If not, which system do you use instead? If you don't use such a structure, I can definitely recommend the de facto standard. Not only does it help to keep your versioning clean and clear, it also helps those who use your software. And it's really easy to set up.
