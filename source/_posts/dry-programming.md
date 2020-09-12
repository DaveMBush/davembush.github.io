---
title: DRY Programming
tags:
  - best practices
  - DRY
  - programming
url: 2519.html
id: 2519
categories:
  - Programming Best Practice
date: 2014-05-29 12:34:00
---

![DRY](/uploads/2014/05/DRY.png "DRY")Today I thought I’d talk to you about the programming principle known as DRY.  As you may know, DRY stands for “Don’t Repeat Yourself” and it shows up in a lot more places than you might expect.  Even when you try really, really hard to not repeat yourself, you end up repeating yourself.  You repeat yourself even when you think you aren’t.  Lots of people repeat themselves.  Do you know of any? Ok.  I think you get the point.  Just like it is silly for me to repeat myself over and over again, it is silly for you to write the same code, or perform the same steps, over and over again.

To drive the point home just a little bit more.  Do you know what the acronym WET stands for?  We Enjoy Typing.  While it doesn’t capture all of what DRY is trying to combat, I think it hits about 80% of the issue.  Have you typed something that is really similar to something you already typed?

<!-- more -->

Copy and Paste
--------------

The most obvious form of something that is WET is code that is an exact duplicate of something you already wrote some place else.  This can be something as mundane as code that is in the same class, to code that exist in multiple classes. In fact, I just ran into this today with some code I was working on.  The person who had written the code had created a local variable to hold a string that he was then passing into multiple methods.  That was good.  This allows us to change the string once instead of changing it in multiple places.

However, he was doing the same thing in three different methods of the same class.  And in two other places he was using that same string as a string instead of using a variable.

Since the string was a reference to a field name, it seemed to me to make more sense to create the variable as a constant member variable in the class and use it in all of the locations.

Copy and paste issue solved.

But what about the case where you need to do something similar but the code is in multiple classes?  In this case, what you’d probably want to do is to create a static class that will have a static property that will return the string.  Better yet, you could put the string in a resource file that you can reference from the static class.

Copy, Paste and Modify
----------------------

The most difficult area in our code to detect that our code is WETter than it should be is when we create code that we’ve done the Copy, Paste, and Modify routine to.  This is because it is similar but not exactly similar.  This is where a huge chunk of our WET code resides.

So as you navigate through your code, you should be looking for code that is similar but not exactly the same and you should be asking yourself this question, “Is there any way I can merge this code so that it only appears once?” One way you might  make this kind of code DRY is by creating a method that takes parameters.  The parameters will let you pass in the stuff that is different while allowing the bulk of the code that is similar to be in one location.

Another way you might deal with this problem is by creating a class with virtual functions that get called by a main method.  Then you can create child classes that have overridden methods that handle the differences in functionality.

Similar Steps
-------------

The hardest type of duplicate code to detect is code that has similar steps.  If you find yourself doing the same thing over and over again, you probably have an area the you either need to deal with, like we dealt with Copy, Paste, and Modify, or you may need to think about creating code that writes your code for you.

This is what techniques like T4 templates were created for.

Similar Code in Different Environments
--------------------------------------

One of the most natural places to deal with duplicate code is in the area of database access.  Typically we have to create tables or stored procedures in SQL, and then to access that code we need to write code in our main development language that mirrors the SQL code.  In CSharp, we create POCOs and CRUD routines.  And then if we are working on a web site, we need to mirror that code once again in JavaScript.

This is a prime candidate for code that writes code.  If you don’t already have something that will do it for you, write some code that will look at your SQL and generate the code you need from that.  DRY says there should be one place that gets modified when a change to the database occurs.  That would be the database.

Not Just About Code
-------------------

But DRY isn’t just about code.

There are things we do every day that have nothing to do with code that are costing us time and money.  In fact, they probably cost more time and money than your WET code because they take longer to perform.

One place this occurs all of the time is in making sure that your code builds.  If you are still doing this manually, you are wasting your time.  If you aren’t doing it at all, that’s even worse.  You may not know for months that your code isn’t building and you will find out at the worst possible time.  And assuming that you have been practicing Test Driven Development, how are you making sure that those tests still work every time you make a change to your code? The obvious solution is a Continuous Integration server that reports back to you that there was a problem.

What about error logs?  You are logging the errors that your system generates, right?  Are you checking the error log?  Manually?  Did you know you could setup a job to email those errors to you as soon as they occur?  No more checking needed, unless you don’t check your email.  In that case, have it send a message to your cell phone or IM you with the error, or at least have it send you a message telling you to check the log.

So, those are some possible places to look for WET code.  Be on the lookout today for places that are WET and spend a little time DRYing things up.
