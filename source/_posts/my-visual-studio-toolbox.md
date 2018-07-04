---
title: My Visual Studio Toolbox
tags:
  - power tools
  - resharper
  - visual studio
  - web essentials
url: 2640.html
id: 2640
categories:
  - Visual Studio
date: 2014-09-18 06:00:00
---

loading...

Last week I started on a blog post about the tools I use and it ended up being about the tools I use for blogging and podcasting. Today I thought I’d work on something a little more generic and look at what’s in my Visual Studio toolbox.

ReSharper
---------

This is my main tool.  I like it so much that I think, at this point, I would be pretty lost without it.  Why?

#### \* On the fly code analysis

ReSharper tells me when I’ve made a stupid mistake long before I’ve actually compiled my code.  It even does this for code that isn’t compiled, like JavaScript.  Catching many of the really stupid errors that I make.

#### \* Automatic Code Refactoring

Not only can it tell you that you’ve made a mistake, but it can also suggest better ways of coding.  I initially learned LINQ by letting ReSharper convert many of my foreach loops into LINQ code. Do you have a variable that you’ve declared a million miles from where it is actually used? ReSharper can move the declaration closer to where it is being used. Do you need to change the name of a method or property, no need to recompile to find all of the places you were using that.  ReSharper can do a lot of that work for you.  The only place where it can’t is if you are referencing the property or method in a string.

#### \* Improved Intellisense

OK.  So, before I got ReSharper, I didn’t know what I was missing.  But not only will ReSharper tell you what is possible, if you type something in, and it isn’t available because you are missing a reference to a DLL, and it can figure out what DLL should be referenced, it will ask you if you want to add the reference. Similarly, if you add a reference to a class in your code and you have not yet added a using statement, you’ll be asked if you’d like to add the using statement to your code.  Sweet! Ever copied code from another file?  When you paste it in, you typically have to add references to using statements as a second step.  Figuring that all out takes time. Resharper ask you if you’d like to add the using statements immediately after pasting the code.

#### \* Coding Standards

I’ve always been a big fan of naming conventions.  ReSharper can help you maintain those coding standards. You can get a free 30 day trial at [JetBrains](//www.jetbrains.com/resharper/).

Web Essentials
--------------

[Web Essentials is a free plugin for Visual Studio](//vswebessentials.com/) that adds a few more tricks to Visual Studio web development. I first found this when I got interested in LESS.  But I’ve become pretty dependent on it.  Many of the features that first show up in Web Essentials eventually show up in Visual Studio.  So, you can think of Web Essentials as Visual Studio vNext. I particularly like the enhancements this plugin adds to my CSS and JavaScript coding.

Productivity Power Tools
------------------------

I originally got the the [Productivity Power Tools](//visualstudiogallery.msdn.microsoft.com/dbcb8670-889e-4a54-a226-a48a15e4cace?SRC=Home) because it has a “Present On” command that lets you bump up the font size of Visual Studio.  Not just the window you are working on.  I use this quite a bit when I present code. But since I got it, I’ve also fallen in love with the feature that puts a squigly in my solution explorer indicating where I have errors in my code.  By turning on JsHint in Web Essentials and using this squigly feature, I’ve been able to keep my JavaScript code clean.

Others
------

So, those are the main tools that I use and that I think everyone should include in their toolbox.  But here are a few others you might want to take a look at.

*   [AnkhSVN](//ankhsvn.open.collab.net/) – Access SubVersion From Visual Studio
*   [SpecFlow](//www.specflow.org/) -  Behavior Driven Development testing tool that works with multiple test runners.
*   [Code Maid](//visualstudiogallery.msdn.microsoft.com/76293c4d-8c16-4f4a-aee6-21f83a571496?SRC=VSIDE)
*   [Code Lens](//visualstudiogallery.msdn.microsoft.com/f85a7ab9-b4c2-436c-a6e5-0f06e0bac16d) This only works with Visual Studio Ultimate.  It provides code metrics to help you keep your code maintainable.