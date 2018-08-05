---
title: 'Automated Testing'
date: 2018-08-05T06:00:00+00:00
author: Marcel Jurtz
layout: post
permalink: /2018/07/automated-testing
categories:
  - programming
tags:
  - development
  - methodology
  - programming
---

Todays focus is on how you can test your applications automatically to detect bugs early and improve your code quality.

I want to introduce you to the concept of automated testing. 
In order to make the content accessible to as many users as possible, 
I will not use code examples and concentrate purely on the underlying idea instead. 
Once you understand them, you will have no problems implementing the concept in the programming language of your choice.

First of all: What is automated testing and what advantages does it offer?

Automated testing describes writing code that tests your actual code. 
Your source code consists of the production code and test code.

Think of how you may test the functionality of your software. 
You start the application and log in with your user account if necessary. 
You navigate to the feature you are working on and perform your tests. 
You repeat that for different possibilities.

You can see from the short description that the whole process is quite time-consuming. 
Each change requires a new run through the procedure and new functions increase the time exponentially. 
And this is exactly where automation comes into play.

Automated tests can be performed at the push of a button and repeated as often as required. 
In addition, they run much faster than manual tests. 
The repeatability means that no more time is required to test existing functionality when adding new features. 
Basically, bugs are better found because no test scenarios are forgotten, 
this procedure strengthens confidence in deployment and allows calmer weekends when a release is to be rolled out on Fridays.

The repeatability of such tests brings another advantage that only becomes apparent at second glance: refactoring, 
i.e. changing code without influencing its functionality, 
is also easier to do. Possible effects of changes on other components of the application can thus be identified directly.

Finally, you automatically think more about the quality of your code as you write it, 
for example, to handle invalid input.

In general, tests are divided into three different categories:

Unit tests test individual entities without external dependencies. 
For example, a single function is tested, but things like database connections etc. are not considered. 
It is only necessary to validate the correctness of the implementation. 
Unit tests are cheap to write and quick to test, but do not provide complete confidence by ignoring dependencies.

Integration tests test an application under consideration of dependencies. 
These tests are often slower because, external resources may have to be accessed, 
but these tests also provide more confidence in the application.

The last category is end-to-end testing. These describe the direct execution of an application via its graphical interface. 
By imitating human behavior, the tests are correspondingly slow and can already be broken by minor changes in the source code.

So much for that. But what exactly is to be tested now? To put it briefly - everything. 
But you should be aware of the so-called test pyramid. From bottom to top you should focus on unit tests, 
then integration tests and finally end-to-end tests. 
The latter, for example, does not make sense for examining borderline cases, but only for core elements of the application. 
Unit tests, on the other hand, are ideal for testing conditions and functional results. 
At this point, however, there are often applications that contain little actual logic and, 
for example, only serve as an intermediate layer between UI and database. 
In this case, integration tests are preferred. 

The basic rule is: deal with gaps in your unit tests with integration tests and use end-to-end tests sparingly. 
However, the exact balance of the three categories always depends on the project.

The approach of implementing automated tests in software projects is usually similar. 
First you are looking for a testing framework for the programming language of your choice. 
For my part I mostly use C#, well-known representatives are MSTest, NUnit or XUnit. 
But you better focus on the basics, not on a special tool. 

Tests are usually written in their own functions, which are executed and evaluated by a Test Runner integrated in the test framework. 
The functions are usually based on the Triple-A pattern (Arrange-Act-Assert) and are named according to a constant scheme. 
This schema usually contains the method name of the function to be tested as well as input parameters or expected result values.

Detailed information on the design of tests can be found on the Internet in sufficient detail for the language relevant to you.

I would like to conclude, however, by referring to a concept based fundamentally on the use of automated tests: Test Drive Development (TDD).

TDD describes a programming concept according to which tests are written before the actual code. 
The idea is to write a test that tests imaginary source code. Since this is not yet available, 
the test will fail (or not compile at all, depending on the language). 
Then the minimum code is written with which the test runs successfully. 
If required, refactoring takes place afterwards. 
The advantage of this concept is testable source code right from the start, 
as well as full code coverage (proportion of code covered by tests). 
Due to the concept of minimalism, TDD often results in a simpler implementation.

You now have a general overview of the idea behind automated tests in software development 
and I hope I have given you enough motivation to try out the concept. 
And if you're working on the details, the keyword 'mock' might also help. 
Mocks are representatives of components of an application that, for example, 
simulate a database connection and thus facilitate integration tests. Happy testing!
