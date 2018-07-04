---
title: Test Driven Specifications
tags:
  - agile
  - tdd
  - test driven development
  - testing
url: 2434.html
id: 2434
categories:
  - TDD
date: 2014-02-25 03:44:00
---

![spider](/uploads/2014/02/spider.jpg "spider")Several years ago, long before the community was actively talking about Test Driven Development, I worked for a short time at a company as a “bug fixer.”  That was my role.  They had hired me because they had some software that was “basically done” but “had some issues.”  It should only take a few weeks.

The first thing they needed me to fix was that the web site was supposed to send out email.  It turns out it was a configuration problem.  But they were so impressed (“the last guy we had in here spent two weeks on that problem and still hadn’t solved the problem.”) that they gave me more and more bugs.

This Is The Job That Never Ends
-------------------------------

The gig that was suppose to be a couple of weeks long was quickly turning into a perpetual job.  Soon I learned that what I was working with was a system that had a lot of bugs, but no one was willing to admit that.  Eventually, frustrated by the fact that this system seemed to have a new bug every day, I asked for the specs so that I could create a test plan.  That’s when I found out the worse news of all about this system.

Lost Specifications
-------------------

They had lost the specs.  Not only had they lost the specs, but they were unwilling to admit this to the client and instead they were relying on the process of fixing the bugs to eventually squash all of the bugs so they could end up with a stable system.

Since I was not yet familiar with the concepts of unit testing or Test Driven Development, I accepted this as the best we could do.  Hey!  At least I was getting paid well.

Oh, but the story gets worse.

The Plot Thickens
-----------------

About three months into this gig, the manager of my project went on vacation which left the project in HIS manager’s hands.  That’s when the poop hit the fan.

The Oracle consultant that was working with me and I were called into the office.

“Why does this system still have bugs?!!”  Oh, he was angry.  That should be all bold and all caps and all underlined.

Well sir, several weeks ago I asked for the requirements document so that I could write a test plan and I was told the requirements were lost.  If we can’t write a test plan, we never will be able to ensure that the system is working the way that it should.

“I want you to write a test plan.”

To which I repeated my need for a requirements document.

We went back and forth with him insisting that I write a test plan and me stating that it could not be done until I finally said, “I think I’ve done all I can do here.”  Walked out of the office, packed up my stuff, went home, and immediately called my recruiter to make sure he got MY version of what happened first.

But, it didn’t have to end like that.

A Better Way
------------

Had I known about test driven development, every time a new bug came in, I could have written a new test, even if it was only unit test and not acceptance test, every time a new bug came in.  Eventually, I would have created not only the test plan, but I would have created the specification, or at least the parts that tended to break, and we would have ended up with a stable system like they thought they were going to get.
