---
title: Raking Leaves and Writing Code
tags:
  - best practices
  - code quality
  - programming
url: 2736.html
id: 2736
categories:
  - Programming Best Practice
date: 2014-11-20 07:00:00
---

So, today I had the task of removing the leaves from my yard, which gave me a lot of time to think because it is a pretty solitary job, even if you have people helping you, because much of the time I was using a leaf blower.  It is pretty hard to hold any kind of conversation when you are using a leaf blower.

And  as I was running the leaf blower, I was thinking about what I was going to talk about today.  And then it struck me, why not just talk about cleaning up the leaves?  I mean if John Sonmez and Scott Hanselman can talk about stuff that isn’t necessarily programming related, why can’t I?

But then I realized, I could talk about cleaning up the leaves in my yard AND talk about programming at the same time.

Think about it.

Why Rake Leaves?
----------------

Why did I clean up the leaves in my yard?  It wasn’t to make my yard look better.  No one can see my yard from the road.  No one would care but my family, and they do care.  But even if they didn’t, we ultimately clean the leaves off our yards because doing that means our grass will grow better next summer by allowing it to see the sun now.  We do it because nasty critters live in leaves.  We remove the leaves from our drive ways and sidewalks so that snow removal will be easier in the winter.  In short, as hard as the work is to take care of our leaves in the fall, the work is worth it because it makes the rest of the fall and winter easier for us  in the long run.

What’s interesting about the first reason, allowing the grass to get more sun during the fall months, would mean that it would actually be better to clean up the leaves frequently during the fall rather than waiting for them to all fall and then rake them.  By the time I get to them, it is probably too late to do any real good because I wait for them to all fall before I remove them.  By that time, the sun shines less during the day and the frost has already kicked in.

How Is This Related to Code?
----------------------------

And that brings me to our code.  We all have code that needs “raking”.  You know you have code that is duplicated all over the place, but have you taken any time to collect all of that code into one place as a function?

How many code smells do you have.  Are your functions long?  Do you have conditional blocks nested more than one deep?  Do you have classes that do more than one thing?  Have you written code that isn’t being used because “we might need it some day”?

As I work with inexperienced programmers, the one thing I notice is that looking for places to simplify code is a skill that needs to be practiced.  It doesn’t come naturally.  Very few programmers assume that as long as the code does what it should, they are done.

And yet, leaving these code smells in place means that the next time you go in to work on the system, you will not be able to understand the code as well as you should.  Because you haven’t let the sun shine on your code, when you get to it to maintain it, you’ll be working in mud instead of nice green grass.

The Challenge
-------------

So today, your job is to find one place in your code that could be made better.  Make it less complicated.  Make it fewer lines of code (without making it hard to read).  Find two places that are doing essentially the same thing and turn that code into a function.  Find two classes that are tightly coupled (highly dependent on each other) and remove the dependencies.  Do this once a day for the next thirty days and see if you don’t find your code easier to work with than it  is today.