---
title: Avoiding Code Complexity
tags:
  - best practices
  - code quality
  - cyclomatic
  - halstead
  - programming
url: 2546.html
id: 2546
categories:
  - Programming Best Practice
date: 2014-06-19 13:27:00
---

![clip_image001](/uploads/2014/06/clip_image001.png "clip_image001")A couple of weeks ago, I talked a bit about how we name things.  Specifically, I talked about the [very bad habit of using the variables i, j, and k as variables in our for/next loops](/i-j-and-k-should-die/). A few weeks before that, I talked about [DRY Programming](/dry-programming/) and the fact that not repeating ourselves extends much farther than most of us normally think when we are thinking about our code. Today I want to continue on the general theme of code quality by discussing code complexity. 

What is it?
-----------

Generally, code complexity is anything you introduce into your code that makes it hard to follow.  But here are a few areas you might look for code complexity.

Do you have REALLY long methods?
--------------------------------

Here’s the deal.  The longer your method is, the more work it will be to keep track of the overall point of the method.  So, you want to keep the number of lines in your method low.  If you are doing too much, when you come back to your code, even you will be confused. I remember when I first started programming.  We only had 25 lines for a full screen, and the rule was, you should be able to see the full function or method on one screen full of code in your editor.  The problem with that metric now is that our screens have gotten capable of showing a lot more code.  It’s a lot like telling a new driver, he should be able to still see the license plates of the car in front of him when he stops behind a car.  Some cars exist where my tiny little Civic would be IN the other car and I’d still be able to see the plates. A better metric would be to use the 7 +/- 2 rule.  Ideally no more than 7 lines of functional code per method.  If you have to, you can fudge it to up to 9, but no more.  This is because the human brain can only deal with about 7 items at a time. I can think of a few times when you might want to break this rule, like when you have a set of conditions that really need to all be in the same place for them to make sense, but it wouldn’t hurt to try to keep to the rule as often as possible.

**Do you have a lot of conditions within one method?**
------------------------------------------------------

When I was discussing the problem with using i, j, and k as variables, I kind of mentioned this, but I didn’t dwell on the subject a lot. You see, the story I told when I was telling you all about the i, j, and k problem violated all of the readability rules.  First, it was using the wrong variable names.  Second, the method was MUCH too long.  And third it had too many conditions. You might think that only if/else statements are conditions.  But so are for/next, while, do/while, and switch statements.  As much as you can, your code should be setup so that you only have one condition per method.  Three at the most.  Again, switch statements might loose their context, but here, I would have one method that handles the switch statement and only one line per case statement.  Each case statement should call a function that does the real work. There is a tool in Visual Studio 2013 that will help you determine how bad your methods are.  You can calculate metrics for a project or the entire solution and it will generate a table of the Cyclomatic complexity of your code.  Look for methods that have a Cyclomatic complexity of over 10 and try to bring them down.  The closer to  zero you get, the better.  Many people suggest that we keep this number below 10 or 11, but this is just an opinion.  I would rather just say look at what you’ve currently got and strive for better. If you have Visual Studio, this option is under the Analyze menu option.

Do you have a lot of operations, function calls, parameters to those calls?
---------------------------------------------------------------------------

What I’m basically talking about here is what’s called Halstead Volume or Halstead Metrics.  What this computes is how complex the code is. For example, if you have five lines of code and a Cyclomatic complexity of ten as everyone suggest, your code may still be in trouble because each line of code is so complex that no one on earth could possibly understand it. We call this self obfuscating code. You probably have some superstar on your team that thinks he’s so hot that he codes an entire function on one line.  The problem with this is that six months from now, neither he nor you will be able to figure out what the code does.  Any fix that will be needed will require an entire rewrite of the code.  That’s exactly what we are trying to prevent. If you are using Visual Studio 2013 Ultimate, you can get the [Microsoft Codelens Code Health Indicator](//visualstudiogallery.msdn.microsoft.com/f85a7ab9-b4c2-436c-a6e5-0f06e0bac16d) which adds the ability to check for all three of the above problems for each of your methods.  If you pay attention to it, it will help you make code that is easier to understand.

Can You Easily Find Code You Need To Maintain?
----------------------------------------------

This is one that is harder to detect, but I thought I’d mention it here because it is a real issue. In an ideal world, we shouldn’t need to use a debugger to track down where the code is that we need to modify.  There are cases where this may be the only solution.  But you should know, for example, that all of your validation code is in this one location.  If you have validation code in multiple locations, you are probably thinking about your code incorrectly. But also cut yourself some slack because this kind of code complexity is not something you are going to notice until late in your project.  You may never see it, but you will certainly recognize it when you run into it in someone else’s code.

Which reminds me of a story.
----------------------------

There was this guy that was working in a cube and he suddenly starts ranting.  “Who wrote this code?!  This is the dumbest code I’ve ever seen.  etc…”  Suddenly he got real quiet and the guy in the next cube asked him, “hey, Joe, what’s wrong?”  Joe replies, “It’s my code.” So even if you don’t pay attention to these issues for your coworker’s sake.  Do it for yourself.
