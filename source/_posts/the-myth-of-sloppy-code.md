---
title: The Myth of Sloppy Code
tags:
  - agile
  - design patterns
  - tdd
url: 4068.html
id: 4068
categories:
  - Programming Best Practice
date: 2016-10-18 06:30:00
---

*   Tightly coupled code runs faster.
*   Tightly coupled code is easier to write.
*   Test Driven Development increases development time.
*   Test Driven Development negatively impacts code design.
*   Knowing the names of design patterns isn’t important as long as you can use them.
*   All my customer cares about is how soon they can have the product, not how clean the code is.

All of these statements, and others like them, are excuses for not writing code correctly.  And you know what an excuse is, right?

> The skin of a reason stuffed with a lie.

<figure>![](/uploads/2016/10/image-1.png "The Myth of Sloppy Code")<figcaption>Photo credit: [dynamosquito](//www.flickr.com/photos/dynamosquito/5866244470/) via [Visualhunt.com](//visualhunt.com) / [CC BY-SA](//creativecommons.org/licenses/by-sa/2.0/)</figcaption></figure>

<!-- more -->    Now, I could go through and argue each of the points above.  But today, I want to look more at the attitude behind these statements.

It’s All in Your Head
---------------------

Man is a funny being.  We make decisions with our hearts and justify the decisions with our brains.  Once you know this, you can hack it to your favor.  But how does this relate to what we’ve been talking about? It is really quite simple.  All of the best practices I’ve mentioned above require work to learn.  We would rather not invest the time to learn how to do these well, and so rather than just saying, “this is not where I choose to spend my time right now” we say things like, “my management won’t let me do that” or “my way is more efficient” or at times, we even resort to vilifying the practice. I have the same issue when I try to convince management that their process needs to change.  It took seven years for me to get one company to move their project management system from email to project management software.  Why?  Because what they were doing “worked” and they couldn’t see that doing something different would work better in any way.

A Skin of a Reason
------------------

So, let’s look at the parts of these myths that are true. First, it is true that while you are learning how to use any one of these best practices, it will in fact, take you longer to write your code. Tightly coupled code is easier to reason about and one could argue is therefore easier to write. Test Driven Development is one of the most difficult things to learn.  And just like the myth that Agile/Scrum/Kanban doesn’t work, the myth that Test Driven Development doesn’t work is largely based on misconceptions about what Test Driven Development is.  Initially, it will impact the time it takes for you to write your code.  But the more interesting argument to me is that testing negatively impacts design.  If you mean that you can’t use the design you are used to using and that design involves writing tightly coupled code.  Yeah, I guess it does. Implementing design patterns is better than not.  It is true.  But that doesn’t mean you shouldn’t learn the names. Yes, your customer cares about how fast you can get the project done.  But, that isn’t all they care about.

The Lies
--------

Just because something takes longer while you are learning how to do it, doesn’t mean it will always take longer.  It probably took you a while to learn how to program proficiently too.  That didn’t stop you did it?  Does it take you as long to program now as when you first started?  Of course not! Let’s look at this from another angle.  As you’ve learned multiple languages, would you say that it made you a better or worse programmer?  Unless you have a very strange perspective, you will probably answer better.  Was the new language hard to learn?  Of course.  Did it take you longer to code using the new language than one you knew previously?  Of course.  Would you say the result of being proficient in both has made it easier or harder to code in both?  Easier, right? Tightly coupled code is easier to write, but as the application grows, tightly coupled code becomes harder to add to and harder to maintain.  It lends itself to code duplication and is more likely to be the code base that needs to be rewritten first. Tightly coupled code is also the hardest to tests and is probably the reason you don’t want to write test for your code.  Because you write tightly coupled code, testing is MUCH harder than it should be.  This leads you to have a firm belief that writing tests is hard as well.  A vicious cycle. And if you believe that tightly coupled code is a proper design, you will believe that tightly coupled code hinders design.  But in fact, writing tests will force you to write loosely coupled code.  So, instead of hindering design, it actually helps it. I used to think not learning the correct names for design patterns didn’t matter.  But the advantage of knowing the proper name for things is that we can all talk about something using the name rather than trying to explain what that thing is with lots of words.  It saves time.  But, there is another much more subtle reason why you need to learn the proper name for things.  You might be calling a design pattern you are using by an incorrect name and confusing the people you are communicating with.  And, finally, learning the names for all of the design patterns will expose you to design patterns you aren’t aware of, broadening your horizons. This last one is, or should be the most obvious.  Getting the project done quickly isn’t ALL your customer cares about.  It might be a relatively high priority, but I can assure you, if you explain to them that getting the project done as fast as possible will mean you have to rewrite the code if they want any changes, you’ll quickly find out that redoing work they’ve already paid for doesn’t sit well with them either.

Benefits
--------

It is a sad, sad truth that by convincing yourself that not investing in these best practices really is best for you and the clients you work for, you’ve also limited yourself to the quality of client you can work for.  The places that pay well expect you to know how to do this stuff.  They believe in good design.  They believe in testing.  They know how to implement design patterns. And guess what, these places also pay better too.  It raises you beyond the level of “commodity programmer” and into the level of “rare gem” How do I know? This past year I’ve been doing a lot of interviewing.  For various reasons, I decided to interview for jobs that pay 25% better than what I currently make.  In the beginning, I did this because I figured it would expose me to people more like myself who wanted to be better.  In the process, I discovered that the interview process is a little harder, but not impossible.  I figured out what I still need to learn.  And, I managed to beat my goal.  Time to notch it up a bit more.

Are You Dead?
-------------

You hear stories all the time about people who can’t find work in our industry.  When you ask, you find out that all they know is something that was hot 10 years ago or longer.  Well, of course you can’t find a job.  And it is no wonder the last company got rid of you.  You provide no value. This past week I heard someone tell me the equivalent of, “I’m not going to learn anything new.  Every time I do, it ends up being hot for a few years and then it is gone.”  I was stunned.  I fear for this guy.  You might as well say, “I’m planning to retire in 10 years or less.” because that is what will happen to you.  Worse, if you stop learning completely, you could end up with dementia or worse. My plan is to program until I’m at least 75.  Longer if I stay healthy enough.  Even if I could retire, I won’t.  What would I do?  I love programming.  I love mentoring other programmers.  I love learning new stuff.

Do hard stuff
-------------

And so what’s the point of all this?  Do hard stuff.  It will make you a better programmer.
