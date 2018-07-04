---
title: 8 Reasons Johnny Does Not Write Bug Free Code
tags:
  - agile
  - bug
  - code
  - scrum
  - tdd
url: 4045.html
id: 4045
categories:
  - Scrum
  - Testing
date: 2016-09-20 06:30:00
---

There have been a number of things that have occurred over the last week that have prompted this particular post.  And for anyone I work with, this is not an indictment of our work place so much as it is an indictment of our industry.  PLEASE don’t take this personally. Some of those reasons will show up in this article.  But the question we need to examine today is why is it so hard to write bug free code.  And I’m not even talking about perfection.  Why is it that we miss the simple stuff?  The stuff that once it is found, we think, “how could we have missed that?!”.  I’m perfectly aware that all code has bugs some just haven’t been found yet.  I’m also aware that no matter how hard I try, the stupid bugs always make their way past my desk. <figure>![](/uploads/2016/09/image-1.png "8 Reasons Johnny Does Not Write Bug Free Code")<figcaption>Photo credit: [~Pawsitive~Candie_N](//www.flickr.com/photos/scjn/3450910519/) via [Visualhunt](//visualhunt.com) / [CC BY](//creativecommons.org/licenses/by/2.0/)</figcaption></figure>

<!-- more --> 

No Training
-----------

Certainly not the only reason.  But at the same time I think this is a core reason.  Our industry really sucks when it comes to teaching computer science.  So much so, that I’ve written articles about how, for the most part, you don’t need a college education to become a programmer.  Maybe if we taught what programmers don’t already know how to do, a college education would be valuable. But what do we do instead?  We teach programmers how to program. Dumb! I think back to my education.  Here’s a basic summary of what I learned:

1.  COBOL, dBase III, JCS, CICS syntax.
2.  Break your code into functions
3.  A bit on how to do requirements analysis.

Guess what?  I could have learned all of that on my own.  How do I know that?  Because I had already taught myself, Basic and C before.  I taught myself every language I’ve learned since.  I was already writing structured code, and still do.  And how we gather requirements has changed and somehow I managed to learn that on my own.  Programming is a learning profession.  It is one of the things that makes it attractive to me. But what didn’t I learn?  At no point did anyone ever teach me how to break my own code.  And while Test Driven Development wasn’t a thing when I was going to school.  I doubt they are teaching it today.  (Let me know if your school did or is.)

Happy Path Specs
----------------

So, the programmers have no training.  But it isn’t just a programmer problem. When is the last time you got a specification from whoever creates them in your organization that had any more than a happy path set of requirements?  But, certainly there are things the system should not do.  I recently had to go asking for required fields and maximum field lengths in an application I was working on.  And that’s the simple stuff.

Not My Job
----------

If you have a QA department, you might be tempted to leave testing to QA.  My personal goal is to make sure QA doesn’t find anything.  At least, not something really obvious. But I know that some programmers get sloppy about testing their code if they know the safety net of QA exist. There is also the problem of QA believing they are the only ones who test.  Strange, but true.  When QA found out I was writing unit test for a pretty complex piece of logic, I was asked, “Then what will be left for me to do?!”  Strange but true. But what if we started working as teams?  For example, what if I could get QA to help me develop my test plan?  What if developing software was a WE activity instead of several silo developers each doing their own thing?

Batch Programming
-----------------

This is one I really don’t understand.  But I know programmers who will write code for hours prior to running it.  Even if you did remember everything you coded, how can you possibly know where a bug is located if you wait that long?  You should be running your code every time you have something different that can be run so that you know what change caused a problem.  And don’t tell me you test every possible condition.  I know you don’t. Programmers who program like this are “Debbie Done” programmers. Why “Debbie Done”? There is this story about a programmer who used to work at one of the companies I worked at in the past.  She considered code done if it compiled and linked. I’m not as good at testing as I would like (yet) and I’m always embarrassed when someone finds a problem with my code.  So, I was shocked one day when I found out that a project manager wanted to give me some work because my code “always works.”  I knew that wasn’t true.  But when I reflected on what he was saying I realized that the difference in how I code and the other programmers he was comparing me to is that I write for a few minutes and then make sure that works before I continue on.

We Don’t Plan to Test
---------------------

Ah.  And here we get a little closer to the truth. What do I mean by planning to test? For any spec you are working on, you should have, written out or coded, a repeatable set of steps that ensures that your code does what it should and doesn’t do what it shouldn’t.  This is what test driven development attempts to steer us toward.  I’m not going to go off on a rant about TDD again here.  But I will tell you that either having a written out test plan prior to coding enables me to ensure that my code does what the people who gave me the specification think it should.  It also forces me to think about ways I might break the code.  I know my code is delivered with less bugs because of this process.  Hopefully, I’ll get better at thinking of how to break my own code. Having a plan helps with the Debbie Done programmer as well as people who code more like me. Even though I code/test incrementally, I still only test the code right after I’ve written it.  Once I think it is working, I don’t go back, even though something else I’ve written may have changed how the code is working.  Having repeatable tests has save me several times.

We Don’t Know what we Don’t Know
--------------------------------

Even if we do everything right, we are still going to miss stuff.  One person can’t possibly figure out all that might go wrong.  It is how we deal with the problems once they are revealed that becomes the issue.  This is where we would, ideally, have the team come up with the test scenarios.

Shame Driven Development
------------------------

I actually heard a project manager say, “Shame on the developer if QA finds bugs.” Really?!  What about “Shame on the BA for not including that item in the requirements.”?  What about “Shame on the product owner for not mentioning it.”? Or what about no shame at all? While shame is a powerful motivator in the short term, it is a sure way to make sure your developers leave. That you only retain highly dysfunctional programmers.  Or that you can only retain programmers who can’t really code. Shame based development can only lead to even more bugs.  Not fewer. At some point I should probably write about the dangers of a shame based culture.   If your organization is using shame to manage personnel.  Get out!

Long Hours
----------

Another way you can kill the overall effectiveness of your team is to make sure everyone works more than 45 hours a week for months or years at a time.  One of two things will happen, if not both. The code will suffer.  Want to introduce more bugs?  Keep everyone working overtime.  A week here or there is a different story. If the code doesn’t suffer, then you are likely to find a lot more socialization, social media activities, and just plain goofing off occurring.  People just can’t work that many hours.  Just because someone is at work for 10 hours doesn’t mean they are working 10 hours.  But hours are easy to measure, so this terrible practice continues.

We Can’t Fix Everything
-----------------------

I’m in a unique position in that I’m currently functioning as a Scrum coach.  This allows me to influence all the areas I’ve discussed.  As programmers, you can only influence your own stuff.  So, my recommendation to you is to concentrate on what you have control over.  Create a test plan prior to writing code.  Once you’ve learned how to do that, work on learning how to code those test so you don’t have to run them manually over and over again.  Do this slowly.  Maybe start with just one test.  Squeeze it into the cracks of your regular work.  Learning to test and learning to code test takes time, but it will make you a better programmer and will ultimately make you a more reliable and faster programmer.  Someday you might just hear that they want to give you an important job because “Johnny’s code always works.”
