---
title: 'Introduction to RegEx'
date: 2019-02-24T06:00:00+00:00
author: Marcel Jurtz
layout: post
permalink: /2019/02/introduction-to-regex
categories:
  - programming
tags:
  - tools
  - development
  - programming
  - regex
---

As a developer you have probably already worked with Regular Expressions. If you're like me and didn't need them too often, you probably googled them up more than you wrote them yourself.

So today I'd like to go through the basics of Regex and show you how to build regular expressions and how to write them yourself.

If you haven't had anything to do with this topic before, this post is still for you, of course. Regular Expressions are a way to check strings against a certain pattern, for example if it is alphanumeric or contains only uppercase letters. Regular expressions are supported by many different programming languages, so you don't have to learn that again with different syntax.

I'd like to recommend the website [regex101.com](https://regex101.com/), where you can test your Regular Expressions and see the individual components in detail.

## Anchor

Regex offers two symbols to mark the start (^) and the end ($) of a string. This makes the following patterns possible:

* ^Hello - Matches any string starting with "Hello"
* World$ - Matches any string ending with "World"
* Hello World - Matches any string containing "Hello World"
* ^Hello World$ - Matches exactly the string "Hello World"

Upper and lower case letters must always be respected!

## OR operator

Next, you can specify more precisely what characters you want to consider valid. Use the square brackets and place all of the allowed characters within them.

* A[BC] - match for any string containing an A followed by a B or C
* ^[AEIOU]$ - match for each string that consists only of one capitalized vowel
* [A-Z] - match for each string containing an uppercase letter

As you can see, the curly bracket always refers to only one sign. For example, if you want to check a string with exactly 5 characters to see if it is alphanumeric, the following spelling is a bit cumbersome:

^[A-Za-z0-9][A-Za-z0-9][A-Za-z0-9][A-Za-z0-9][A-Za-z0-9]$

## Quantifier

With quantifiers you can limit the number of possible occurrences. There are the following possibilities:

* \* Zero or more
* \+ One or more
* ? Zero or one
* {} Explicit quantity

Applied concretely, the whole thing can look like this:

* ABC* - match for each string containing AB, followed by none or more C
* ABC+ - match for each string containing AB, followed by at least one C
* ABC? - Match for any string that contains AB and follows a C or no C
* ABC{3} - match for each string containing AB followed by "CCC"
* ABC{3,} - Match for each string containing AB followed by 3 or more C
* ABC{3,6} - match for each string containing AB followed by 3,4, 5 or 6 C

If you want to apply the quantifier to more than one character, you can parenthesize the respective ones, the quantifier will then refer to their contents: AB(CD){3} - Match for each string containing AB and following CD 3 times thereafter. If you want to use a character that has a special meaning in Regex (for example *), you can mark it in your expression with an escape character (backslash, \).

## Classes

In my example above, I already used "A-Za-z0-9" to check if a text is alphanumeric. Classes provide a way to check groups of characters more easily. The following classifications are available:

* \d - Matches a digit
* \w - Matches alphanumeric characters and underscores
* \s - Matches Whitespace (incl. Tabs & Linebreaks)
* . -  Matches any character

You can capitalize any of the classes to negate their meaning.

## Flags

RegEx expressions are usually found within two slashes. After the closing slash, the scope of the pattern is defined, whereby the following options are available:

* g - (Global) No return after the first match. The search for the pattern continues after the find
* i - (Insensitive) Not case-sensitive when testing according to a sample
* m - (Multiline) ^ and $ denote start and end of a line instead of the whole text

## Advanced functions

To put it briefly: There are several additional options for defining regular expressions. Since I would like to limit this article to the basics, I will not go into the topic in detail here, but will show some practical examples. But if you are interested in the topic, I have some keywords for your own research:

* grouping, named groups & capturing
* bracket expressions
* lazy matching

However, you will also come across other topics during the search, but in my opinion the basics are sufficient for the time being. You can go into depth as soon as you really need it, otherwise it is very likely that you forget the theory before you get an opportunity to apply it.

## Practical examples

Finally, here are a few examples that cover the most common requirements. Of course, you can find almost anything, like formatting credit card or phone numbers by a quick google search. But now you should be able to write something like that up all by your self.

#### Alphabetical (Upper- and Lowercase):

`^[a-zA-Z]*$`

#### E-Mails:

`^([a-zA-Z0-9._%-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,4})*$`

#### URLs: 

`^(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$`

#### Dates (MM/DD/YYYY, 20. and 21. century): 

`^((0?[1-9]|1[012])[/](0?[1-9]|[12][0-9]|3[01])[/](19|20)?[0-9]{2})*$`
