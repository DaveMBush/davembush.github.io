---
title: Don’t Comment Out That Test
tags:
  - tdd
  - test driven development
url: 2478.html
id: 2478
categories:
  - TDD
  - Testing
date: 2014-04-24 11:38:00
---

![DeleteTDD](/uploads/2014/04/DeleteTDD.png "DeleteTDD") First, at the suggestion of one of my friends who now works at [SmartBear](//smartbear.com/), I’m going to experiment with creating audio of my post going forward.  Obviously this will lend itself to some of my post more than others, but I think it is worth experimenting with. Those of you who’ve been with me since the early days remember that I use to produce [YouTube videos](//www.youtube.com/user/davidmbush) occasionally.  I haven’t done that in a while because the post production process for videos takes so long.  I’m hoping that I don’t run into the same problem with producing audio.  Anyhow, the point of creating the audio is that many of you don’t have time to read, but would have time to listen to something on the way to work.  Assuming this experiment works out, you should be able to listen to the content on this blog during your commute, or anywhere else you are likely to listen to audio.  If you'd rather listen to this post, you can scroll to the bottom of this post and click the "Play" button.

<!-- more -->

Today’s Problem
---------------

What I want to talk about today is the problem anyone who has been using test driven development for a while has run into.  That is, you’ve got a suite of test that are running pretty well but one day, you are working in crunch mode and a set of previously running test stop working because of some change you’ve made recently. What are you going to do?

Crunch Time
-----------

Remember it’s crunch time.  Maybe it was some bug you fixed that caused the test to stop working.  This bug is critical and you need to get the fix into production as quickly as possible.  To further complicate issues, your process won’t let you put anything into production that the [Continuous Integration](/make-your-test-work-for-you/) system you have in place says won’t build.  And by “Won’t Build” I mean either, it won’t build or the test won’t past.

The temptation in this situation is to just comment out the tests with the promise that you’ll come back to it later.  But you and I both know that once you’ve commented out the tests, you are going to forget about them and move on.

It’s Like A Car
---------------

I had a manager once who said you can compare everything to a car.  So, let’s use that to illustrate this problem.  You are driving down the road and the check engine light comes on.  What are you going to do?  Well, the prudent thing to do would be to find a garage as soon as possible and find out what the issue is.  Maybe you need oil.  Maybe it is something much more serious.  Maybe it is just a flaky light.  But you won’t know until you get it checked out.

What you don’t want to do is to put a piece of tape over the light and continue on as though nothing were wrong.  Even if the light is faulty, what you want to do is to get the light fixed so that it can warn you properly when something really IS wrong.

Commenting out test is a lot like putting tape over the check engine light.  Maybe the test are broken but the application really does work.  Maybe the spec changed and you need to change something about the test.  But if you comment out those test, then you don’t really know that the application works.

How Committed to Test Driven Development Are You?
-------------------------------------------------

It is at this point in the test driven development process that you really show how committed to test driven development you are. If you comment out your test because they aren’t working, then your development process really isn’t driven by tests.  Your tests are driven by development. If you didn’t think the bug was fixed yet, would you still be thinking about moving this to production?  If you could comment out essential code in your application to fix the bug, would you do that? Of course not.  And the reason is clear.  If you did that, someone is likely to notice.

The reason we are more likely to comment out a test is that it is unlikely that anyone will notice and the faster we get the bug fix into production, the better we look.

But that’s really just a lie we tell ourselves.  The truth is, if you comment out those test and later a bug crops up that those test were designed to find, or worse those test were designed to catch a bug that previously existed, someone will notice that too.  And then how smart will you look? Crunch mode tells us something about our character.  Something about what we really believe.  If you are willing to comment out test, what else are you willing to take a short cut on?  If you are willing to comment out test, why did you write them to begin with? Someone who really believes that  testing is part of development will fix the test bugs along with the application bugs the same as they would fix a bug in the application that fixing a bug produced.  It’s all part of the same code base.
