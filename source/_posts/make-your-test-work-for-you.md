---
title: Make Your Test Work For You
tags:
  - CI. Continuous Integration
  - tdd
  - test driven development
url: 2470.html
id: 2470
categories:
  - Continuous Integration
  - TDD
date: 2014-04-18 11:53:00
---

![TDD-CI](/uploads/2014/04/TDD-CI.png "TDD-CI") So far we’ve been talking about creating test as part of the development process.  If all you ever used those test for was to make the design of your systems better, you would already be far ahead of most of your peers.

But now that we have test, we might as well get as much mileage out of them as possible.  To do that, we are going to run our test every time we make a change.

Now, you COULD run your test every time you make a change manually.  But who has time for that?  Certainly not me.  I doubt you do either.  And even if we did have time, there are better things we could be doing with that time if we could have those test run for us by some other process.

<!-- more -->

Continuous Integration
----------------------

For this we need a continuous integration server.  Here’s what you want to make sure your CI server is able to do.

*   Run your unit test
*   Run your integration test
*   Run your acceptance test

To make sure it can do that, it will also need to be able to pull the source code from version control every time a new change is committed and build the new version.  I would bet that IF you have a CI server at all, this is all that it does.

It needs to be able to notify  the developer who put the change in that the change caused a problem.  Ideally, it wouldn’t even allow  a change to go into your version control system if committing the change would cause the build or the test to fail.

By implementing a CI server you are well on your way to ensuring that whatever is in version control can be released without causing any problems.

CI Setup
--------

Since unit test are the core of your system, you’ll want to make sure that you run these test with every build.  But your integration test, and especially your acceptance test are going to take a lot longer than is practical to run as part of the process that runs with each check in.  I’ve been working on one scree of a system and so far I have about 30 hours of acceptance/integration test using Selenium to drive 3 different web browsers.  Even if I narrowed the test down to one browser, that’s about 10 hours of test.  I don’t want to wait 10 hours to find out that my build worked (or not) and the changes that one of those test will fail is pretty small.  So, I’ve scheduled half of my test to run on one day and half of them to run on another day.

With a good CI setup, you can be sure that changes you are making are not breaking code you’ve already confirmed is working.
