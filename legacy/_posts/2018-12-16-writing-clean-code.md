---
title: 'Writing Clean Code'
date: 2018-12-16T06:00:00+00:00
author: Marcel Jurtz
layout: post
permalink: /2018/12/writing-clean-code
categories:
  - development
tags:
  - programming
  - software
  - development
  - methodology
---

There are lots of articles and even books about the art of writing clean code. But when exactly is code actually to be considered clean? Today I'd like to give you a brief overview of how you can assess and improve your code quality. I should perhaps note that writing good code, and therefore this article, is independent of the programming language used and is rather methodical in nature. However, for more detailed information, I recommend that you really get a book on the subject. The standard advice on this is certainly "Clean Code" by Robert C. Martin.

First of all: What is clean code? Everyone probably has their own definition here, but there are a lot of overlaps. A key characteristic of clean code, for example, is good readability and maintainability. Why should the readability of your code be good? If you think about how much time you spend writing code and how much you spend reading it, you will probably get a ratio of 1:10. So if you make your code readable, you'll make future work easier.

Everyone probably knows the following scenario: You are pressurized by deadlines and tend to write "Quick and Dirty" code. Although the problem is solved quickly, in the long run this is the more costly approach. If this code has to be extended later, it will be much more difficult than if somebody thought about a suitable strategy before implementing it in the first place.

You alone are responsible for your code. Imagine another profession: For example, in the food industry or as a doctor or nurse. If someone else told you not to wash your hands from now on to save time - would you do that? Probably not. And that's exactly what makes your business professional. But what can you do to actually make your code more readable?

Use self-explanatory names. Variables, classes or methods have to directly expose what they do. Name methods and variables so that a class does exactly what you expect it to do after reading the source code. Also, avoid using abbreviations in your naming to make reading easier. Oh, and even if it feels tempting to name all variables, classes, methods, and the like after different Pokémon, don't. Clarity is unfortunately more important here than being funny. Also, make sure you name similar concepts the same way. Suppose you write an API that receives data and performs simple CRUD operations - name all functions that perform similar tasks identically. For example, choose Insert, Add, or Append for creating new objects, but don't mix these names.
 
Your code should speak for itself. This does not mean that you have to avoid comments completely. However, comments are often used when the purpose of the code is not clear. The problem here is very often that the code is changed, but the corresponding comment remains, and thus provides wrong information. So if you are in the process of writing a comment, think about whether it really makes sense or whether you should revise the code.

There is another point regarding naming, especially for methods and functions: The single-responsibility principle. A method should do exactly one thing, and it should be done well. If you have a method that creates and returns an instance of a class with default values, that method should not add the object to a collection of objects of that class as well. Each method has exactly one task. If your method takes 5 arguments, it might probably do more than one thing.

Write tests. I confess myself guilty of doing too little of this, but write automated tests. Not just to sleep better before releases. Write unit tests to validate individual parts of your application and combine them using integration tests. Of course, you can also add other test categories like UI testing, but I think you should start small: Anything is better than nothing.

Practice, practice, practice. This list is not complete, not at all. But it is certainly a good start. To write clean code, it's all about practice. Look at other people's source code. Ask developers to look at your code and give you feedback. You will notice if your code is good and internalize the important characteristics all by yourself.  