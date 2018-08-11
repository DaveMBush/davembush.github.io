---
title: How to Really Screw Up an Angular Project
tags:
  - angular
  - best practices
  - cli
url: 4523.html
id: 4523
comments: false
categories:
  - Angular
date: 2017-12-12 06:30:38
---

We all know about best practices.  But what does it take to really mess up a project?  Well, for starters, you do EVERYTHING wrong.  You don't just ignore one or two best practices, you ignore them all.  By evaluating the mess you can get yourself into by ignoring best practices, I think we can all learn better why these recommendations exist. <figure>![](/uploads/2017/12/2017-12-12.png "How to Really Screw Up an Angular Project") Photo on [VisualHunt](//visualhunt.com/re/f81060)</figure>

<!-- more -->

Don't Use the CLI
-----------------

I can grant a pass if you aren't using the Angular CLI because you started your project before the CLI became viable.  But, by this point you should have already converted your project over to use the CLI or be making plans to move to the CLI.

Why is this a problem? 

Because 99% of the developers you are going to find will expect that when you ask for an Angular developer, what you mean is that you are looking for someone who can write Angular code using the Angular CLI.  This brings with is a certain number of expectations about how your code is laid out.  Some of these are outlined below.  But just as a way of generalizing, if your code doesn't look like it started as an Angular CLI project, you are going to slow down any future developers you hire.

Ignore Naming Conventions
-------------------------

Ignoring naming conventions may seem trivial, but naming conventions add clarity.  The reason we name component files as components is so we know they are all part of a component.  Reducer file names should be named `foo-bar.reducer.ts`.  Not naming the files in a way that is clear reduces the ability to maintain the code in a clear and efficient way. 

Maybe you've come up with your own naming conventions for your organization.  This is better.  But still this is not best.  This means that you probably won't be able to use the CLI to scaffold out your code and any future developer is going to have to learn your way of writing Angular instead of the industry standard way of writing Angular. 

But the absolute worst thing you can do is to not have any standard or to mix standards.  This is confusing and just looks ugly.

Cluster Code by Function Instead of Feature
-------------------------------------------

The other thing you might be tempted to do is to cluster your code by function.  That is, you might want to group all of your components together.  And then all your services under another folder.  Maybe put your reducers under one folder and your effects under another. 

Trust me, this just adds fuel to the argument that "NgRX is confusing".  Group all your similar NgRX code together and put all of your code together grouped by feature.  If you've named the files correctly, you won't need the directories to keep things sorted out and you won't be forced to use one module for your entire project.

Use one Module
--------------

Maybe you don't know any better.  Or maybe it is because you've violated the rules above.  But no matter the reason, if you end up putting all your imports, providers, and declarations in your app module, you'll soon see just how ugly this looks.  One for the whole application violates the single responsibility principle.  You want a module per feature at the very least.  I often create modules just to provide an additional level of granularity that further implements the single responsibility principle.

No Lazy Loading
---------------

Yes, even if your application has only one route.  You want to lazy load your code so that a different bundle gets created for each route.  Done well, you can make changes to one route without impacting any of the others.  When you deploy the new code, your end user should only have to reload the route(s) that changed.  Without lazy loading, when you redeploy, they'll have to reload an entirely new set of files.

Embed Colors and Fonts in Component CSS
---------------------------------------

You should have a theme file, or an application level CSS file that defines the fonts, and colors that should be used throughout your application.  The only CSS that should be included at the component level is CSS that is necessary to layout the html within the component.  That is, position information.  If you are putting color information in, or specifying a font size or font, you are probably doing it wrong. 

Why is this an issue?  

Well, let's suppose that someone decides that all of your warnings should be a different color.  If you can make that change in one CSS file, that is going to be a lot easier than looking through all the CSS in all of your components to make sure you found every place the color needs to be changed. 

Don't repeat yourself makes just as much sense in CSS as anywhere else in your code.

Mix Template Files and Strings
------------------------------

As you should know by now, you can create the HTML templates and the CSS templates either by using strings in the TS file or by using separate HTML and CSS files.  You should use one style and use it consistently.  I've seen one project where they were using a mix of both and they had at least one file that was using a string for the HTML but still had the HTML file next to the TS and CSS file.  That's just confusing.  Don't do that!

Don't Remove Dead Code
----------------------

As we work on code, we might create a variable, or use an import that no longer is needed.  The linter is really good about telling us what code is no longer needed.  Use it and keep you code cleaned up.

Don't Stay Up to Date
---------------------

Angular is progressing at a pretty fast rate and the Angular CLI is as well.  I realize that it isn't always possible to keep the version you are working on up to date with the latest tools.  But not keeping your tools up to date for several months at a time is also something you want to avoid.  The sooner you update, the easier the update will be. 

Will the update break something?  

Yes, that is likely.  You should plan that into your work flow.  Otherwise, you'll get to the point where it will take so long to update, you'll never get approval to do it because it will take too long.
