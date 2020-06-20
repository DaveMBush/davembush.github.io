---
title: Is JavaScript Broken?
tags:
  - concatenation
  - javascript
  - quirks
url: 2820.html
id: 2820
categories:
  - Javascript
date: 2015-01-15 08:05:00
---

![IsJavaScriptBroken](/uploads/2015/01/IsJavaScriptBroken.png "IsJavaScriptBroken")

I read a post this week that was essentially a rant on the way JavaScript handles concatenation.  It states that JavaScript is in someway “broken” (without actually using that word) because JavaScript does not work the way this person expected it to.

<!-- more -->

Here is a rebuttal.
-------------------

JavaScript works the way JavaScript was designed to work, and the way many other languages work.  The fact that it does not have the Do\_what\_I\_was\_thinking() function is a separate issue (which so far as I know, no language has yet).

The main statement I have to take issue with is:

> The + operator is known to most of us as a symbol for addition, so it does not make sense to use it in \[the instance where we want to concatenate strings\]

Oh?  Really? How about C++, C#, Java and the various flavors of VB?  Each of these languages have an overloaded + operator that performs concatenation.

Then as you read through the article you discover that the REAL issue is the fact that you can’t concatenate numbers by either using the concat() method or by using the + because numbers are … well … numbers.

But here again I have to point to the fact that JavaScript works similar to other languages that are not type-safe.

For example VBScript, VB6 using variants and probably others have this same basic issue.  If you are going to use the plus operator for concatenation and for math, you have to have rules about when it will be used for each.  Since numbers are typically ADDED together and strings are typically CONCATENATED, it make sense the the default behavior would be for those behaviors to be primary in those instances.  In the case where you want to concatenate two numbers, the obvious choice would be to somehow force them to be strings instead of numbers.  And in all of the languages I know of where this is needed, putting an empty string in the chain of numbers you want to concatenate is how you achieve this behavior.

What is humorous is that this person went  through a long series of commands to illustrate how painful it is to actually coerce numbers into being concatenated and then shows a “simple” method they’ve created to create a concat() method for numbers that essentially amounts to what anyone who knows JavaScript well would do inline:

var newValue = “” + number + number

So, no JavaScript isn’t broken any more than any other language.  It has a defined set of rules that it follows.  Saying that it is broken is like saying English is broken.  And frankly, I could make a much stronger case for English being broken than I could for any programming language being broken including JavaScript.
