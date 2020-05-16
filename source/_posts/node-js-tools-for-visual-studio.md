---
title: Node.js Tools For Visual Studio
tags:
  - express
  - javascript
  - node
url: 3020.html
id: 3020
categories:
  - Node.js
date: 2015-04-02 06:00:00
---

![NodeJsInVisualStudioProjectList](/uploads/2015/03/NodeJsInVisualStudioProjectList.png "NodeJsInVisualStudioProjectList")

Several weeks ago now, I started learning [Node.js](//nodejs.org/).  Why?  Well, for a couple of reasons.  First, all the cool kids are using Node.js.  Second, I wanted to use [Istanbul](//gotwarlost.github.io/istanbul/) to get an idea of how well my javascript code is covered by test and that runs under Node.js.  Third, [Node.js is going to show up in the next version of Visual Studio](//blogs.msdn.com/b/webdev/archive/2015/03/19/customize-external-web-tools-in-visual-studio-2015.aspx).  And finally, I just like to learn new stuff.

<!-- more -->

Node.JS
-------

So, I started by installing node and just working in Visual Studio as though my node project was a web site.  It works, but it isn’t pretty.  But it did get me familiar with some basic concepts like using the node package manager (npm) to install what I needed to get Istanbul running.  For those of you who aren’t familiar with npm, it is basically NuGet for node.js.

Istanbul JavaScript Code Coverage
---------------------------------

For those of you who are interested, I used the instructions [here](//ariya.ofilabs.com/2013/10/code-coverage-of-jasmine-tests-using-istanbul-and-karma.html) for getting Istanbul working locally.  They should probably be updated because the example there doesn’t have a good configuration example.  I filled in the rest of what I needed to know from the [Karma site](//karma-runner.github.io/0.12/).

Node for Visual Studio
----------------------

So then I had heard that work was being done on a plugin for Visual Studio 2013 that would allow me to work on Node projects from within Visual Studio.  I found out about this first from [Scott Hanselman’s blog](//www.hanselman.com/blog/IntroducingNodejsToolsForVisualStudio.aspx).

So I went  to the [plugin site](//nodejstools.codeplex.com/) and got the download and installed it.  This is all pretty straight forward.  Don’t forget, you’ll ALSO need to install node.  So, don’t forget that step.

OK.  Now that you have the NTVS installed (that’s what they call the plugin) what do you have? Well, you have several new project templates that you can use.  That’s what.

![NodeJsInVisualStudioProjectList](/uploads/2015/03/NodeJsInVisualStudioProjectList1.png "NodeJsInVisualStudioProjectList")

Interactive JavaScript
----------------------

But that’s not all you get, you also get an interactive Node.js window that you can get to from the Tools menu (Tools –> Node.js Tools –> Node.js Interactive window) or by pressing the keyboard shortcut, Ctrl+K, N.  Inside this window you can execute JavaScript on the fly: ![NodeJsInteractiveWindow](/uploads/2015/03/NodeJsInteractiveWindow.png "NodeJsInteractiveWindow")

Node Package Manager in Visual Studio
-------------------------------------

Or install other node packages:

.npm install yourPackageNameHere

Don’t forget the leading period.  That’s the indication to the window that it needs to do something different from executing javascript in the window.

Even as I’m writing this, I’m seeing that there is a lot more here than I’m actually telling you now.

Starting a New Project
----------------------

Now, the best way of learning something is by creating some kind of product.  Even if it is for your own use.  So, the next thing I did was to try to create a project.  I have a specific project in mind that should use MongoDB for the database (I’ve been meaning to learn NoSQL for a while now) and since Express seems to come with NTVS, I guess I’ll use that for my web server.  Oh and Angular for the front end.

Since I was creating a web site, I thought, I should create a new web project that uses node.  In fact, it shows up  in the list of web sites types that you can create.  However, I got an error when I did that, and I don’t see anything in the documentation that says I should be able to use those templates or that I need anything extra to use them.

However, if you use the project options, you can create a web application.  I decided going with newer is better than older, so I’ve created a “Basic Node.js Express 4 Application”.

The Fun Is Just Beginning
-------------------------

And now, this is where the fun begins.  Where’s my HTML?  What’s all this “template” stuff?  And how do I install Angular?  Or do I do that manually since it isn’t a server side thing? So much to learn.  But, that’s for another post.
