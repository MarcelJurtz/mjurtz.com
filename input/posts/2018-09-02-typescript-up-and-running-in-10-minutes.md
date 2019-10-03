---
title: 'TypeScript - Up and Running in 10 Minutes'
date: 2018-09-02T06:00:00+00:00
author: Marcel Jurtz
layout: post
permalink: /2018/09/typescript-up-and-running-in-10-minutes
categories:
  - development
tags:
  - development
  - software
  - programming
  - typescript
  - javascript
---

You are aware of JavaScript, but have never really made friends with the language? 
Today I would like to show you a first insight into the JS-SuperSet TypeScript, 
so perhaps you can give JavaScript a second chance.

TypeScript is a SuperSet of JavaScript. SuperSet means that all functionalities of JS are both supported and extended. 
As the name suggests, the main feature of TypeScript are strongly typed variables. But more on this in a moment.

TypeScript is developed by Microsoft and was initiated by MS employee Anders Hejlsberg, who had already designed C#. 
TypeScript files typically end with the extension .ts and can be converted to JavaScript using the TypeScript Compiler (tsc). 
Did I also mention that TypeScript is completely open source?

## Setup

I use Visual Studio code to design the example in this article. Of course you can use any other editor as well. 
My application is also based on NodeJS, which you can download from the project's website or via the package manager of your choice. 

In short, NodeJS is a way to run JavaScript on the server side. This allows you to use your applications without a browser. 
After you have installed Node, you should install the typescript package, which gives you access to the TS compiler. 
You can use the Node Package Manager (npm) to install packages. 
To do so, open a terminal window of your choice and enter the command *npm install -g typescript*.
The -g flag specifies that the package should be installed globally instead of only in the current directory, 
so that the TS compiler is now available to you from anywhere.

## Hello World!

Let's start with an example. How could it be otherwise, we will develop our Hello World application in TypeScript. 
Next snippet is all we need:

```typescript
var msg:string = "Hello World!";
console.log(msg);
```

As you can see, variables are declared with the keyword var as in JavaScript. 
However, the type of variable follows the name of the variable, separated by a colon. 
TypeScript provides three basic types: string for strings, 
number for numeric values (always represented by a 64-bit floating point number) and boolean for logical values. 
Here, too, we will go into more detail in a moment, but now we would like to know how to get our program up and running.

Currently you should have a directory containing a single .ts file. 
To generate a JavaScript file from it, we call the TS compiler from a terminal and pass our script to it. 
In my case it looks like this: tsc main.ts.

If everything is ok, a new file will be created with the same name as our .ts file. 
Now, however, with the.js extension. The new file contains the following content:

```javascript
var msg = "Hello World!";
console.log(msg);
```

Admittedly, there is hardly any difference here. 
You can see, however, that the typing is no longer applied. The new JavaScript file can now be used with Node. 
Here, too, the terminal is used to invoke the program: node main.js.

This creates the "Hello World!" output we are looking for.

The TypeScript compiler also helps you to avoid errors. For example, if you try to assign a text to a variable of type *number*, 
you will receive the following error message when compiling:

```bash 
main.ts(3,5): error TS2322: Type '"test"' is not assignable to type 'number'.
```

Depending on which editor or plugins you are using, you will be notified during development. 
VS code, for example, shows you directly the displays the corresponding error message.

Of course, this is only the tip of the iceberg, but we want to start small.

Your first piece of TypeScript is ready - but you certainly can't do too much with it yet. 
My goal for this article was to give you a basic idea of TypeScript and give the language a chance. 
With Microsoft in the background, TypeScript has a strong backing company and offers great potential, especially for C# developers, 
since learning the language should be much easier than using JavaScript directly.

If you would like to learn more about TypeScript, the language has extensive documentation. 
I have also planned a follow-up article for the next steps.
