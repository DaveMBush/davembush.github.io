---
title: The Fallacy of Motion
tags:
  - productivity
  - tdd
  - testing
url: 3164.html
id: 3164
categories:
  - TDD
  - Testing
date: 2015-07-23 06:00:00
---

![image](/uploads/2015/07/image1.png "image") I had this thought this past week that we tend to believe that if we are in motion, we are accomplishing something.  That being busy somehow equals being productive.  And then I started thinking about how this is almost universal.  It doesn’t just impact how we program, which I’ll get to eventually, but it impacts all of our life.

Let Me Illustrate
-----------------

Have you ever driven to some location and had your favorite GPS system route you multiple ways?  How many of you take the route that is, according to your GPS, longer, just because the roads are 65mph instead of 45mph?  Or an even better example.  You hit a traffic jam and, even though ever indication is that you’ll be out of the traffic in a few minutes, you take an alternate route simply because the traffic is moving on that other road.  In both cases you probably know that you won’t get to your destination any faster, in fact you may get to your destination slower, but you still opt to take the moving route instead of the one that isn’t moving or is moving relatively slowly.  I’ve even heard people say… shoot I’ve even said… “I want to take this route because it moves faster.”  I like FEELING like I’m getting to my destination faster even if the reality is that I’m not. And then there is the issue of your job.  How many of you have to look busy for your peers to think that you ARE busy?  But as programmers, how true is that?  Unless you do some kind of unskilled labor, I doubt it is true of most people who work.  And yet, because we are all paid by the hour (yes, even those of you who are salaried are still trading hours for dollars) there is a perception that, somehow, if we are not in motion, we are not really working.  But is that true? I remember hearing a story about a programmer who spent half his day “staring at the ceiling” who was twice as productive as any of his peers.  Why is that?  Because he spent half his day thinking about the problem before he ever started writing a line of code. I remember back in the day when I was coding on 8088 computers, a compile and link cycle could take up to 10 minutes.  Because I recompile and test every time I’m made a runnable change, I had a lot of 10 minute breaks.  I used this time to think about the problem I was working on.  This was not intentional, it just happened naturally.  I think it made me more productive than if I were just churning code all day long at the speed our computers work today. And then there is this side project I’m working on.  I tend to work on it a half hour at a time as I have time.  Between my main gig that takes up 40 hours a week, and some side gigs that I have, I have a very small amount of time to work on this project.  Some days I don’t work on it simply because I’m too tired.  But sometimes, even when I’m not sitting in front of my computer, I’ll sit on my sofa doing nothing and think about the project.  It is in those times that I figure out where I want to take the project next, or how to solve a problem I’ve run into.  That time thinking, even though there is no motion, has made the resulting code that much better.

The Fallacy of Motion and Testing
---------------------------------

I’m sure you saw this coming, but one has to ask, is it the fallacy of motion that prevents us from testing our code? At my main gig, I have to admit, I’m bored.  There are a lot of reasons for this.  But one of them is that I spend a lot of time waiting for integrations tests to run to verify that my code is still working.  To be clear, I have way more unit test than integration test at this point.  But with the changes I’m currently making to the system, I’m more likely to find issues via the integration tests than I am from the unit tests. Compare this to a project I’m working on for another client where I’m primarily writing new code.  I’m a lot less bored.  The day zips by.  I FEEL more productive. And yet, the degree of certainty I have that the code I wrote for my main gig works as I designed it to work is a lot higher than the code I’m currently writing because I have more tests. This past week those tests actually showed me that my change wasn’t working entirely the way I wanted it to.  I probably would not have found it any other way.  I’m not even sure the people testing my code would have found it.  They aren’t those kind of testers.  And so I’ve proven to myself one more time that testing, while it feels slower, really is not just the right thing to do, but ultimately the faster thing to do. Think about what would have happened if I didn’t have these tests in place.  I would have released the code, eventually the bug would have revealed itself, and I would be left scrambling to fix the bug.  As it is, I was able to fix the bug in a somewhat leisurely manner because I found it early. And yet, I don’t FEEL productive.  As much as I’m convinced that Test Driven Development is the right way to go, I still struggle with this Fallacy of Motion.

We Don’t Have Time to Test
--------------------------

I heard this again this past week.  “When you are working on a project with a tight deadline, you can’t always test because you don’t have enough time.” What?! Is this the Fallacy of Motion at work again? I would argue that it is precisely BECAUSE you are working under a tight deadline that you NEED to write test. Think about this, you have this tight deadline.  Great.  So you are not going to think clearly.  You are going to rush.  You will write incomplete code.  Your methods won’t verify input parameters.  You’ll have null pointer exceptions.  Your code will do what it is supposed to do, but will it not do what it shouldn’t do?  How will you know?  And who is going to catch the bug? Well we know the answer to that last question.  The people using your code will catch the bug.  This will make you look like an idiot.  You are because it is something that should have been caught.  It will also mean that you’ll need to track down the problem, fix the problem, and re-deploy the software.  And this is faster?  Faster than what?! Of course, “We don’t have time to do it right, but we always have time to do it over.”

Slow and Steady
---------------

So, slow down.  Do things right.  Remember the story of the tortoise and the hare.  The race doesn’t always go to the fastest but the one who is steady and persistent.  The one who doesn’t cut corners.  The one who consistently produces solid code.
