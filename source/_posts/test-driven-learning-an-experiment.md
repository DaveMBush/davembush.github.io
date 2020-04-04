---
title: Test Driven Learning - An Experiment
tags:
  - learning
  - tdd
url: 3856.html
id: 3856
categories:
  - TDD
date: 2016-03-24 07:30:00
---

As I mentioned last week, [I’ve been learning React JS over the last month or so](/react-js-and-associated-bits/). Up until the start of this project, I would learn a new framework, and then I would try to paste in Test Driven Development after the fact.  I would use the excuse that because I didn’t know the framework well enough, I wouldn’t be able to properly write tests for it.

But this time, I decided to do something different.  What if I wrote tests for my demo application as I was learning this new framework?  My reasoning was that learning how to test code written in the framework was just as important as learning the framework.

What follows are the lessons I learned from this wildly successful experiment.

![image](/uploads/2016/03/image-4.png "Test Driven Learning") Photo credit: [twm1340](//www.flickr.com/photos/tom-margie/1538953234/) via [Visualhunt.com](//visualhunt.com) / [CC BY-SA](//creativecommons.org/licenses/by-sa/2.0/)

<!-- more -->

I Learned the Framework Better
------------------------------

As I’ve written before, most people don’t test because they’ve never learned how.  When you are learning a new framework, in a lot of ways you start out at that same place all over again.  Yes, you’ve got all of your previous testing experience to fall back on, but in a lot of ways, because you don’t know the framework, you don’t really know how to properly test the code you are writing.  So, doing anything takes a lot more time than it would if you just wrote the code and ignored testing.

But what I learned by forcing myself to write test as I wrote the demo – either before, as TDD should be practiced, or immediately after – is that this all helped me learn the framework better than if I hadn’t.

For example,

*   I stopped thinking of JSX as HTML and instead saw it as code with objects that can be mocked.
*   I saw that the Flux system worked best as a “notify with data” system rather than a “notify and pull” system.
*   I was forced to fully understand Flux
*   I was more inclined to enforce the Single Responsibility Principle both at the View level and with Flux

I Learned How to Test the Framework
-----------------------------------

This probably seems obvious.  But this gets back to the problem with my normal way of learning a framework.  Let’s take something like Angular as an example.  I know Angular 1.  I can get a pretty respectable application put together in it.  But, because I didn’t learn it using TDD, I would be the first to admit that my ability to unit test an Angular application is pretty weak.  I’m actually planning to go back and rework my Angular 1 and Angular 2 reference applications using TDD just to get that experience.

I recently started working with EXT again.  This time with version 6.  Guess what I spent the first couple of days learning how to do?  That’s right.  “What’s the best way to write code using EXT 6 that makes the code testable?”  How did I answer that question?  I wrote tests.

I learned:

*   If you use the ViewModel and ViewController and never have either call the View directly, you can create highly testable code.
*   Unit tests in EXT are best written using PhantomJS instead of JSDom.  Although, I’d still like to go back and try to make JSDom work.

In React JS, I learned:

*   View components are easier to test if they only do one thing.  Favor composition over monolithic view component.
*   Stores (the part responsible for CRUD actions) can also be EventEmitters

I Didn’t Develop Bad Habits
---------------------------

This one, to me, is probably the most significant reason for me to repeat this process in the future.

Here’s the deal.  Every other time I’ve learned a new framework or language, I’ve learned it from a “let’s just get something working” perspective.  If I concentrated on testing at all, it was after I got it all working.

The problem with this method is now I’ve learned how to get something working, but I haven’t really learned the best way to organize my code so that it can be tested or swapped out later if I need to.

By writing my code using TDD, I will have to write the code in a way that is testable the first time.  This means that while it may have taken you over twice as long to get that first demo application up and running, all future applications will be written in a more efficient, more testable, and easier to maintain.

I Learned How Testable the Framework Really Is
----------------------------------------------

If you read last week’'s article, you know that the testability story for React JS is what really got me excited.  None of the other frameworks I have used have been testable starting at the view level.  This is one of the areas where Angular could improve.  I wish I had leaned Angular 1 using TDD because then I would have either figured out how to test decorators on directives or I would have found out that it is only possible under the certain conditions.

So far, I think React is incredibly testable.  Although I have not had a chance to do some of the things I’ve done with Angular yet.

I Learned Where the Holes Are
-----------------------------

This goes back to learning the framework better.  Because I was testing along the way.  Because I implemented code coverage.  I discovered that the code coverage story is the weakest link right now.  This is because Istanbul in combination with ES2015 syntax, which is the preferred React JS JavaScript syntax, does not work well together yet.  So, you’ll never get 100% code coverage no matter how hard you try because the transpilers that convert your ES2015 code to ES5 code add in conditions that will never get triggered, which show up in Istanbul as uncovered conditions.  This is probably more a reflection of the state of JavaScript right now than any of the tools involved.  Had I been willing to convert all of my React JS code to ES5, I would have been able to get 100% code coverage all the way down from the view to the server.

Conclusion
----------

So, I invite you to try it yourself.  The next time you are learning a framework, learn ALL of it by testing it along the way.
