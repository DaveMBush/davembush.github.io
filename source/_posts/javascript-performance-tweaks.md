---
title: JavaScript Performance Tweaks
tags:
  - book
  - javascript
  - performance
url: 2612.html
id: 2612
categories:
  - Javascript
  - Programming Best Practice
date: 2014-08-21 09:00:00
---

[![](//ws-na.amazon-adsystem.com/widgets/q?_encoding=UTF8&ASIN=059680279X&Format=_SL250_&ID=AsinImage&MarketPlace=US&ServiceVersion=20070822&WS=1&tag=davmbusnetapp-20)](//www.amazon.com/gp/product/059680279X/ref=as_li_tl?ie=UTF8&camp=1789&creative=390957&creativeASIN=059680279X&linkCode=as2&tag=davmbusnetapp-20&linkId=JAJ5BQZSEX6VSPFJ)![](//ir-na.amazon-adsystem.com/e/ir?t=davmbusnetapp-20&l=as2&o=1&a=059680279X) So, I started reading [**High Performance JavaScript**](//www.amazon.com/gp/product/059680279X/ref=as_li_tl?ie=UTF8&camp=1789&creative=390957&creativeASIN=059680279X&linkCode=as2&tag=davmbusnetapp-20&linkId=JAJ5BQZSEX6VSPFJ)![](//ir-na.amazon-adsystem.com/e/ir?t=davmbusnetapp-20&l=as2&o=1&a=059680279X) recently and I thought now might be a good time to give a summary of what I've learned so far. Up until recently, I wasn’t all that concerned about performance.  But last year I started working with EXTjs, which allows you to build desktop like apps in JavaScript using pretty much nothing but JavaScript and now performance is a much bigger concern.  It isn’t like the old days where I was supplementing the page with JavaScript.  Now, HTML is supplementing the JavaScript.  Even if you aren’t working on anything quite as intense as what I’m working on now, you are going to want to learn how to write JavaScript in a performant manner because JavaScript is become the language of choice for so much.  It isn’t just a web thing anymore but it shows up in mobile development and servers. The only thing that might make learning how to optimize your JavaScript irrelevant is if you wait until a really good binary compiler comes out that is available everywhere.  Not something I see happening any time soon, much as I’d like that. OK, so that being said here are the two main optimization tweaks I’ve learned so far.

Local Is Faster Than Anything Else
----------------------------------

If I could boil all of chapter two into one rule.  This would be it.  Any time you are accessing a variable, make sure you turn it into a local variable if you are going to use that data more than once in your local scope. Why is this? Because every time you access a variable the JavaScript runtime engine has to do a lookup to find out where it is.  The first place it looks is in the local scope.  From there it works its way out to the various scopes it has access to. And be careful thinking that just because it is a property, you can ignore this rule.  Even properties take more time than going after a local variable.  At a minimum  it has to do a lookup for the variable that represents the object and another lookup for the property or method attached to that object. In a compiled language, this generally isn’t necessary because these references are all figured out when we compile the code.  So you’ll need to get your head out of “compiled language mode” and back into scripting mode.

Avoid Accessing the DOM
-----------------------

Yes, I know, this is why we write JavaScript code on the client in the first place.  At least, most of the time it is. But the point of this rule is that we want to avoid accessing the DOM any more than we absolutely have to. Why is this slow? Well, the first reason is the generally the DOM engine and the Scripting engine are in two separate spaces.  So you kind of have to pay a toll every time you access the DOM from JavaScript. Second, manipulating the DOM is slow.  Historically this was not such a huge problem because the DOM was only expected to be rendered once. So what can you do to help your performance? First, once you’ve accessed a DOM element, cache the result on the JavaScript side  of the fence so you don’t need to pay that toll again. Second, don’t do anything in multiple calls that you could do with one. Pretty simple.

What Else Is In The Book?
-------------------------

1.  Loading and Execution – How to optimize getting the code on the client side to start with.
2.  Data Access – All about variables
3.  DOM Scripting – Optimizing access to the DOM
4.  Algorithms and Flow Control
5.  Strings and Regular Expressions
6.  Responsive Interfaces
7.  Ajax
8.  Programming Practices
9.  Building and Deploying High-Performance JavaScript Applications
10.  Tools – Profilers, etc.

I’ve learned a lot in a few days and it has changed how I look at my code.  The book is really easy to read and even though it has a 2010 copyright on it, and lacks metrics for current browsers, 99% of the material still applies. Check out [**High Performance JavaScript**](//www.amazon.com/gp/product/059680279X/ref=as_li_tl?ie=UTF8&camp=1789&creative=390957&creativeASIN=059680279X&linkCode=as2&tag=davmbusnetapp-20&linkId=JAJ5BQZSEX6VSPFJ)![](//ir-na.amazon-adsystem.com/e/ir?t=davmbusnetapp-20&l=as2&o=1&a=059680279X) today!