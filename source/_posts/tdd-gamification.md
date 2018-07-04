---
title: TDD Gamification - Turning Test Driven Development into a Game
tags:
  - agile
  - extreme programming
  - gamification
  - paired programming
  - tdd
url: 3049.html
id: 3049
categories:
  - TDD
  - Testing
date: 2015-04-23 06:00:00
---

![ge-gam-018](/uploads/2015/04/ge-gam-018.jpg "Test Driven Development Gamification")When I was in college, there were some guys I hung out with who played this game called “Questions” which they got from some book.  [Actually, it was a play](//en.wikipedia.org/wiki/Questions_%28game%29). Anyhow, the basic rules are:

*   You can’t answer a question with a statement
*   You can’t hesitate or make a false start
*   You can’t repeat a question that has already been used
*   You can’t ask a rhetorical question
*   You can’t ask an unrelated question.

There was also [this podcast at DotNetRocks](//dotnetrocks.com/default.aspx?showNum=1111) where they were talking about a beer app and how they had added game elements to the app by adding badges for various types of beer to get you out of your comfort zone.  Maybe there is one for “My first beer that I liked” because I’ve yet to find something I like.  But give me a good Merlot! All of this got me to thinking about how we might turn Test Driven Development into something of a game. There are several people who have already addressed the Gamification of TDD

[TDD and the Gamification of Testing](//effectivesoftwaredesign.com/2011/11/21/tdd-and-the-gamification-of-testing/)
--------------------------------------------------------------------------------------------------------------------

This article talks mostly about how TDD has many elements of good gaming inherent in it.

*   Because it makes testing more like programming.  Well, what he actually says is that it raises testing to the level of complexity the programmer is familiar with.  But in reality, I think this is because it makes testing something that can be coded.
*   Because TDD is measurable.  The test either succeeds or fails.
*   Because TDD gives you immediate feedback.  Or at least a well written test gives you immediate feedback.

[TDD: The Gamification of Programming](//benvanik.tumblr.com/post/20702406947/tdd-the-gamification-of-programming)
------------------------------------------------------------------------------------------------------------------

This article provides an interesting perspective of someone trying to program using TDD for the first time.  You get the normal complaints, like “this takes too long” along with acknowledgement of some of the benefits.  But what he realizes is that the process has made programming more like a game than about the creative problem solving most of us got in to programming for in the first place.

[Gamify TDD](//www.tdddev.com/2014/09/gamify-tdd.html)
------------------------------------------------------

This is the first article I found that, instead of saying that TDD is already a game, like the two above, actually ask how we might formally turn TDD into a game because it already has many elements of a game.  But he never actually says HOW he would do that.

TDD Gamification
----------------

Putting this all together.  Here is how I would actually make TDD into a game.

#### The rules:

*   The game is played in pairs.  (Paired programming)
*   Game starts when programmer 1 writes the first test.
*   Once a failing test is written, programmer 2 writes JUST ENOUGH to make the test succeed.
*   Once a test succeeds, programmer 2 writes another test, and programmer 1 writes just enough to make it succeed.
*   Play alternates until no one can think of another test that will force more code to be written.
*   Programmer loses points if he is caught writing more code than is necessary to pass the test.
*   Programmer loses points if he is caught writing code that has dependencies embedded in it.
*   If a dependency is required, the test writer and the code implementer will collaborate in the method of dependency injection to be used.
*   All points lost by one programmer go to the programmer who caught the mistake.

#### Embellishment:

If code is found to have a bug in production, both programmers responsible for the code loose a point, regardless of how many lines of code are impacted.  One point per recorded bug. I’m assuming that most shops have more than 2 programmers.  On any given day, programmers would be paired up for the day of programming.  They are responsible for turning out bug free code.  Because we are tracking the code over the long term, we will be able to see just how effective each programmer is at eliminating bugs. In fact, if you are in an environment that can’t quite stomach having your programmers paired up, I would suggest keeping track of the code that wasn’t done under the game.  This would be the “house score” compared to an individual programmer’s score. The only thing we have left is some way of being able to account for productivity.  I mean, if I write no code, I’ll have no bugs, right?  And all things being equal, the more productive I am, the more bugs I’ll have.  So how do we measure this? Here is what we can’t do:

*   Lines of code – you can’t measure based on lines of code because that has been shown to be an inaccurate measure of productivity.  More lines may just mean you are not very efficient.
*   Hours worked – [Rapid Development](/rapidDevelopment) has statistics in it that show there is a factor of 10 difference between good programmers and bad programmers.

#### Just a suggestion:

Since short methods are desirable, what if we measure number of test relative to number of methods relative to number of classes.  This would enforce the single responsibility principle. T = Tests M = Methods C = Classes 1/((T/M)/C) = Productivity Score Therefore,

*   If I worked on 3 classes each with 6 methods and each method has 2 tests, I end up with a productivity score of 1.5
*   If I had one class with 18 methods each with 2 tests, I end up with a productivity score of .5
*   If, on the other hand, I put everything in one method, and one class and had the same 36 tests, my productivity score would be .02
*   If you write no code, your productivity score is 0.

That’s just a simple way to calculate.  You may choose to use cyclomatic complexity in some way if you have that number easily available to you. Well, that’s my rough sketch of how we might be able to turn TDD into something of a game along with being able to prove to ourselves and management that it really does produce better code. How else might you embellish this or change it so it works better?  Let me know in the comments below.

#### Other Test Driven Development Resources

*   [Test Driven Development: by Example](/testDrivenDevelopmentByExample)
*   [Professional Test Driven Development w/ C#](/professionalTestDrivenDevelopmentWithCSharp)
