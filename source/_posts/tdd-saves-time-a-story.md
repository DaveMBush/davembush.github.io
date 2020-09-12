---
title: TDD Saves Time – A Story
tags:
  - programming
  - tdd
  - testing
url: 2504.html
id: 2504
categories:
  - Programming Best Practice
  - TDD
  - Testing
date: 2014-05-22 12:40:00
---

![TDDSavesTime_AStory](/uploads/2014/05/TDDSavesTime_AStory.png "TDDSavesTime_AStory")I recently had an experience writing code that proved to me, once again, that using Test Driven Development really is faster than the way I have been working. You will remember a couple of weeks ago I presented [a strategy for creating test scenarios where we need to test storing data to a database from a web page](/automated-web-…tional-testing/). 

<!-- more -->

Well, recently, I had a chance to use that strategy.  In that article, I talked about first testing that the data that we were sending back to the server was actually coming back correctly to the web site.  By doing this, you don’t have to track down where the problem is.  Is it in the database save routine or did it not even get from the client to the server?  This part worked pretty much as expected and my code for that worked right away so I can’t say that I saved a whole lot of time.

But the part that did save me a TON of time is the second half.  Saving the data to the database without having to use the web site to create the data I wanted to save.

While I was testing the client side, I saved the data to a JSON string which I used to create an object that represented the data that had been sent back.  Then in my unit test, I recreated the object from the stream and sent that into my save routine.  Once the save was done, I reloaded the object from the database.  Now I have the original object and the saved object which I can compare.

And the comparison showed me that the save wasn’t quite working the way I had expected.  I actually had to go through several (10? 15? 20?) iterations of fixing and testing before I got to a point where my test was succeeding.  It took about a day to get everything working.  Imagine if I were using a manual method to test this.  Launch the web site, fill out the form (it’s a pretty long form with a lot of data) save the form, reload the form, remember what I had  filled out, verify that everything got saved.  I easily saved half a day using Test Driven Development.

If you are  still on the fence as to the value of implementing test prior to writing code, I would encourage you to try it.  Just try it for 30 days.  Yes, it will be hard getting started.  Yes, it will FEEL like it takes more time to write the test first.  But as you make this part of how you develop code, you will start to see for yourself how many benefits you can realize by implementing this best practice.
