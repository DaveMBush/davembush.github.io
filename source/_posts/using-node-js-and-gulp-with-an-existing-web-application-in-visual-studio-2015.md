---
title: Using Node.js and Gulp with ASP.NET in Visual Studio 2015
tags:
  - asp.net
  - gulp
  - javascript
  - node.js
  - visual studio
url: 3354.html
id: 3354
categories:
  - ASP.NET MVC
  - Node.js
date: 2016-01-21 08:30:00
---

As I’ve written before, [I’m using AngularJS a lot](/tags/angular-js/) recently to write the client side of my web applications.  As I’ve gotten to the end of my current project, I found myself needing to implement cache busting and while I am at it compression.  But because I’m [using a regular HTML page to serve up the shell for my single page application](/asp-net-angular-js-html5mode/), using the regular ASP.NET on the fly compression wasn’t going to work for this application.

But there are a lot of tools in the Node.js space that will work.  Would it be possible to wire node.js and Gulp with ASP.NET in my existing web project? It turns out you can.

Although, at this point, it isn’t as straightforward as most other things in Visual Studio.

![image](/uploads/2016/01/image-2.png "image")

<!-- more -->

Installing Node.js
------------------

I’m assuming that you’ve already installed Visual Studio 2015.  If you did that, you already have Node.js installed.  The problem is, it isn’t the most recent version.  So, what you want to do is to install the most recent of version from the [Node.js site](//nodejs.org) manually.

Once you have Node.js installed, the next thing you will need to do is that you’ll need to configure Visual Studio to use the version you installed instead of the version it installed.  To do this, navigate to “Tools” > “Options”.  In the resulting dialog, find the “Projects and Solutions” > “External Web Tools” leaf in the tree control and then add the directory to your newly installed Node.js installation to the top of the list of paths to external tools. [![image](/uploads/2016/01/image_thumb.png "image")](/uploads/2016/01/image-3.png)

NPM
---

To use Gulp, you will need to install Gulp using the Node Package Manager (npm).  There are several places where I got stuck.

First, I tried to install gulp using the Node Interactive Window (CTRL-K,N).  This works fine when you want to install something globally, but when you want to install something with the --save or --save-dev option, you will get the error message: `Please specify a valid Node.js project or project directory.` It took me several tries before it finally dawned on me that it wasn’t just asking “What project do you want to install this in?”  It was asking “What Node project do you want to install this in?” We don’t have a node project, so this will never work.  What you will need to do instead is that you’ll need to shell out to the command prompt, change to the project directory, and then type your npm commands.

Productivity Power Tools
------------------------

You can shell out to the command prompt much easier if you install the [Visual Studio Productivity Power Tools](//visualstudiogallery.msdn.microsoft.com/34ebc6a2-2777-421d-8914-e29c1dfa7f5d?SRC=VSIDE)  Once these are installed, you can right click on the project you want to install npm packages into and select “Power Commands” > “Open Command Prompt…” from the menu.

NPM init
--------

Much like NuGet’s packages.config file, NPM uses a json file to keep track of what should be installed.  To create this file, run the command `npm init` and answer the questions.

Next, in Visual Studio, click the “Show all files” icon in Solution Explorer, find the package.json file that you just created with the `npm init` command, and include the package.json file in your project.  This will cause it to be part of your commit so that anyone who pulls your code down from version control will have the packages installed automatically.

Install Gulp
------------

Once you’ve shelled out to the command prompt, you’ll need to type in the following two commands.

``` shell
npm install gulp -g
npm install gulp --save-dev
```

If you open your package.json file now, you will see an entry for gulp.

Create a Gulp task
------------------

The final step in this process is to create the actual Gulp job.  To do that, all you need to do is create a gulpfile.js file in the root of the project.

Inside the gulpfile.js file, add the following:

``` javascript
var gulp = require('gulp');

gulp.task('default', function () {
    // place code for your default task here
});
```

And now you have a default task for gulp installed.

Make Gulp Part of the Build
---------------------------

The final step here is that we want to make gulp part of the build.  Otherwise, what’s the point? In Visual Studio 2015, this is really rather simple.

From the main menu, go to “View” > “Other Windows” > “Task Runner Explorer”.  You should end up looking a a sub windows in Visual Studio that looks like this: ![image](/uploads/2016/01/image-4.png "image") In gulp, you might have multiple tasks in a gulp file. We only have one right now, “default”.  If you right click on that, you will see that you can bind that task to one of the four bindings on the right.

That’s all you have to do.  Now the gulp task “default” is bound to a specific build step.  You can do all kinds of file processing with this which we may cover in a later post.  But for now, you can at least get it all wired in.
