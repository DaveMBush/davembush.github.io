---
title: Debugging Software
tags:
  - debug
  - debugging
url: 3139.html
id: 3139
categories:
  - Programming Best Practice
date: 2015-06-25 06:00:00
---

![spider](/uploads/2015/06/spider.jpg "spider") It amazes me how rare the skill of debugging software is.  It is even more amazing to me that after programming for over 27 years now, I still have trouble with this.  So, as a checklist for myself, and as a possible help to others along the way, I offer you “How to Debug Your Code” 

Is It Plugged In?  Is It Turned On?
-----------------------------------

Another way of stating this is, “Validate your assumptions.”  I learned this phrase while I was traveling with a high school music group as a sound tech.  I was one of two guys who was responsible for making sure the sound equipment got from one site to the other and got setup at each site and was ready to go in time for the group to practice at least once before the performed. The thing with sound equipment is, there are lots of connections.  Lots of places to miss.  There were many times that we would set everything up and test it all out and there would be no sound, or partial sound.  I’m ashamed to tell you how many hours we wasted tracking the problem down only to find that something wasn’t connected.  An amp wasn’t turned on.  A plug had come out of the wall.  Cables had become disconnected. And the same sort of thing happens with our code.  You deploy code to the wrong directory and wonder why it isn’t showing up in the browser.  You deploy code to the right directory, but you forgot to get it from version control before you did.

Read the Error Message
----------------------

This is especially true with .NET but even when all you have is an error code, the error message is useful. In .NET, when you get an error message, it all but tells you what line of code you need to fix.  And even if you don’t understand the error.  You can copy and paste the error into a search engine and you are likely to come up with multiple possible solutions.  Even if all you have is an error code.

Sometimes the Error Message Is Hidden
-------------------------------------

I was just working on some code last night after putting in a very long day.  My code wasn’t working so I finally decided to pull up the debugger.  In this case, the code was JavaScript and I’m running it in Chrome because what I’m writing is a Chrome plugin.  When I finally loaded the plugin with the debugger active, it was then that I saw the console log message telling me that my variable wasn’t defined.  Talk about stupid mistakes.  My variable was code I meant to enter as a string, but because I didn’t put quotes around it, the JavaScript engine saw it as an undefined variable.

Run the Debugger
----------------

Once I’ve eliminated the obvious, the next thing is to pull out the debugger and set break points.  Now, this can be tedious.  How many times have you decided to step over when you should have stepped in?  Arrrgh.  Now I have to start over again. Well, here’s how I speed things up. At the top level function.  I initially step over until I find out what line is causing the problem.  If you have an exception that is being thrown, this is easy to do because you just have to wait for the exception.  If you are getting a wrong answer, it may be a bit more difficult because you’ll have to verify your expected results along the way.  But the point is, you need to find out what line is throwing things off or you will be there all day, stepping through code one line at a time. Once you know which line in that function is causing trouble, you step into that line on the next run and step over each line in the lower level function until you find out what line is causing a problem there. Rinse, lather, repeat, until you find the problem.

Step In To Libraries
--------------------

This goes back to validating your assumptions, but I call it out here separately because we never think that we’ve made an assumption about something that is supposed to be a “black box” when in fact, we have. Back in the day when I was doing a lot of Visual C++ coding, I learned a lot about how MFC worked by stepping into the MFC code.  So much so that I became the expert in my office regarding fixing C++/MFC related bugs. Today I’m doing a lot of JavaScript coding.  For the last two years I’ve actually done more JavaScript than C#.  Specifically, I’m writing a lot of EXTjs code.  The thing about EXTjs is that the framework is pretty complex.  I would argue too complex.  But that’s another post.  What I want to point out here is that I’ve fixed a lot of issues with my EXTjs code by stepping into the EXTjs framework code, as painful as that was, to find out why my code was behaving the way it was.

The Search Engines Are Your Friends
-----------------------------------

This one should be obvious.  But, unfortunately it isn’t. The hardest part about searching is trying to figure out what other people may have used to describe the problem so that you can find the answer that is just sitting out there waiting for you.  I’m convinced that as intelligent as the search engines have become, using them is still a kind of art.

Explain the Problem to a Co-Worker
----------------------------------

You would be amazed at how often the act of describing a problem to someone else brings the obvious solution to light.  If you’ve ever done this, you know that often, in the middle of describing the problem, you’ll says, “… oh, never mind.  I know what the problem is now.” Which leads to my alternative solutions…

Explain the Problem to Your Coffee Cup
--------------------------------------

Why?   Because it isn’t your co-worker who helped you solve the problem when you explained it to him.  It was the act of explaining it.  Once you’ve articulated the problem well enough to explain it to someone else, that act actually often hands you the solution.

Step Away For a While
---------------------

I don’t know why it is, but for some reason, we seem to think that if we aren’t coding, we aren’t working.  But you can’t work non-stop as a programmer for 4 – 8 hours without taking a break.  And banging your head against a bug when you’ve already done that for 15 minutes probably isn’t going to produce any better results if you do it for another 4 hours. Yeah, you may try a bunch of stuff.  You may look busy.  But the fact of the matter is, you might be better off finding something else to do for an hour or so and then coming back to the problem once you’re brain has had a rest from it. Well, those are my tips for now.  What tips do you have?  Leave them in the comments.  

### Other Places Talking About Debugging Code

*   [Conditional Breakpoints](//dailydotnettips.com/2015/05/19/use-conditional-breakpoints-with-methods-return-value-in-visual-studio-2015/)
*   [Debugging – The 9 Indispensable Rules](/debugging9)
