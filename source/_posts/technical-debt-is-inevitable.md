---
title: Technical Debt Is Inevitable
tags:
  - agile
  - scrum
  - technical debt
url: 2665.html
id: 2665
categories:
  - Programming Best Practice
  - Scrum
  - TDD
date: 2014-10-16 06:00:00
---

Whoa there Dave.  What are you talking about?  Have you given up the fight? You who have preached the TDD religion.  You who’ve struggled to get organizations to adopt naming conventions, to use version control systems and to use project management software.  The same guy who has implemented continuous integration on his current project?  What’s this world coming to?

<!-- more -->

No. Relax. I haven’t given up.  In fact it is precisely because technical debt is inevitable that we need to implement all of the above.

Danger
------

But there  is a danger in believing that if we were to implement all of the best practices in the book, or that could ever be devised, that technical debt would simply vanish.  And the sooner some of you realize this, the less stressed out you will be.

You see, no matter how good of a programmer you are today, tomorrow you will be a better programmer.  That is true for all of us.  We are all doing as good of a job as we possibly can today.

A Story
-------

This reminds me of a story I heard once.

It seems there was this programmer who had to make a change to some code and, like we all do, the first thing he did was to try to wrap his head around the code he was looking at.  A few minutes into this common exercise, he starts exclaiming:

*   Who wrote this code?
*   This is the worst code I’ve ever seen.
*   The guy who wrote this can’t even really call himself a programmer!

And then, it got unusually quite in his cubicle.

His friend in the next cube calls over the wall, “Hey John, you OK?”

And John responds, “It’s my code.”

Now, the reason this is a funny story… well, it is a funny story for me anyhow… is because this happens to us all of the time.

I think I spent the first 5 years of my career looking at code I wrote six months ago and thinking, “What was I thinking when I wrote this?”

Even today, 26 years in, I have code I wrote six months or more ago that I know I need to rewrite.  The only reason I haven’t yet is because I want to make sure I have a full test harness around it before I tweak it.

Here’s the point.
-----------------

We are all getting better at what we do.  What we would do today isn’t what we would have done six months ago.  The person who wrote the code you are looking at was doing the best he could at the time.

Therefore, we should just plan on code being “wrong.”

This has two explicit implications to how you relate to your code and your co-workers.

### Don’t be surprised by bad code

First, you should not be surprised when you find bad code.  You should instead be shocked when you find well written code.  Since even you write bad code, you should be gracious when you find bad code that isn’t yours.  You don’t want to be the guy in the story  I just told.  You might have done the same thing today if you knew only what the person who wrote the code you are looking at knew at the time the code was written.

### You should expect that your code will be broken in some way.

This has been probably the hardest thing for me to get control of.  I have, historically, been one to deny that my code has a bug.  I’ve taken it as a personal insult, or an assault on my character when someone finds a bug in my code. 

Until recently.

Once I was able to internalize the fact that the person reporting the bug was not upset, that the only person who expected me to be perfect was me, and that being wrong was part of being human, I was able to calm down a bit.

You see, none of us are all knowing.  Most of us program for what the code is supposed to do and don’t think about what it shouldn’t do (which is where most of the bugs occur).  The spoken/written language is an imprecise communicator, even if you develop a dictionary for your project.  You’ll never get all of your terms defined.  You just don’t know what you don’t know.  And therefore there will be bugs.

Maybe you’ll think, “I should have known that!”  Well, yes, maybe you SHOULD have.  But the fact is, you didn’t.

When before I used to deny that the bug could even exist, my reaction now is, “Hmmm, wonder what’s going on there.  OK, well, put it in the issue tracker and I’ll get it fixed.”

No drama.  No conflict.  No denial.  No blame.  Just deal with the issue.

This is what it means to be Agile
---------------------------------

And you see, this is the beauty of the Agile methodology.  Agile assumes we aren’t going to get it right the first time.  It assumes humans are poor communicators.  It assumes that programmers aren’t going to understand the problem the first time they try to come up with a solution.  It assumes technical debt is inevitable.

Isn’t it time that you do too? Or am I the only one who has suffered with this problem?
