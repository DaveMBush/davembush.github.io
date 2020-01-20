---
title: How to Establish Peace to the QA vs Dev Battle
tags:
  - best practices
  - project management
  - QA
url: 4039.html
id: 4039
categories:
  - Scrum
date: 2016-09-13 06:30:00
---

Have you ever noticed how, when QA reports a “defect” developers tend to bristle?  I first noticed this in myself a few years ago.  Now that I’m functioning as a Scrum coach, I’m noticing it in others.

Is there a way to have some kind of quality checking in our code that doesn’t make the whole process feel so adversarial?  I think so.

I believe there are some adjustments that need to be made organizationally and personally that will bring these two groups together.

But first, why does this problem exist in the first place? <figure>![](/uploads/2016/09/image.png "How to Establish Peace to the QA vs Dev Battle")<figcaption>Photo credit: [m01229](//www.flickr.com/photos/39908901@N06/9129574323/) via [Visual hunt](//visualhunt.com) / [CC BY](//creativecommons.org/licenses/by/2.0/)</figcaption></figure>

<!-- more -->

How The Battle Started
----------------------

### How to Insult a Programmer

When I was growing up, my sister was very direct.  If she thought you were ugly, she’d tell you so.  OK.  Maybe it wasn’t quite that bad.  But one day, I remember she said something to the effect of, “I didn’t say anything that wasn’t true.”  And both my mom and I said, “Yes, but it isn’t what you said so much as how you said it.”  In fact, you can say the same words in two entirely different ways.  Two sets of inflections.  And what they mean can change drastically.  Sometimes just changing the words you use can change the meaning.

But, to a programmer, no matter how you say, “Your code has a bug,” they will probably end up hearing.

“Your code sucks!” You see.  Programmers really do care about their code.  At least the really good ones do.  And while it may seem very silly, most programmers get offended when you suggest they’ve written buggy code.

This, you see, is the core of the problem.  Once you understand this, the fix becomes rather obvious.

### Don’t Call It a Bug

I think nothing has done more damage in the field of programming that the fact that we call software problems “Bugs.”  It is such an ugly word.  When is the last time, other than a Pixar movie, when we’ve thought of bugs as something we would want to welcome?  We might as well say, “Hey, I found some shit in this program!” When I got my Scrum certification, our instructor asked a simple revealing question.  “How many of you think people are doing their best to do the right thing?”  That’s not exactly what he asked, but that’s what he meant.  If you believe that all evil in the world is intentional, you’ll come away believing that all programmer intentionally put bugs in their code.  And while I do know of a few cases where this has happened.  Most of us try our best to write perfect code.  When we don’t it is because we didn’t think of the situation and code for it.

If on the other hand, you think everyone is trying to do their best, why treat defects in software as something evil? To paraphrase Scott Hanselman, “Most people are not nearly smart enough to be as evil as you act like they are.”

### Most Bugs are a Specification Problem

Weather you have a formal specification or an informal specification, my observation is that most “Bugs” that show up in code are a result of either 1) the specification being misunderstood or 2) the specification being incomplete.

Yes, there are a few places where neither of those are true and the programmer clearly missed the mark.  But, even then, assuming they missed the mark rather than assuming the specification was unclear will go a long way in making the programmer more receptive to the fact that the code needs to be changed.  Just the fact that we call a defect in our programs a “Bug” reveals and colors what we think about who’s fault

What Programmers Can Do
-----------------------

### It’s Not Personal

Listen gang.  Bugs are not a reflection of your personal character.  And even if someone thought it was, that doesn’t make it true.  At worse, it means you might have some stuff you still need to learn about how to program well.  OK.  We’ll never be perfect.  Think about this, while you are writing the code, your compiler, or runtime, tells you you’ve done something wrong quite frequently.  But as soon as a human tells you something similar, you take it personally?  That’s pretty wacked.

Here are a few tips I’ve learned:

*   Emotions are learned responses.  This means your negative response to bugs can be retrained.
*   Just because someone says something about you, or disapproves of you personally, doesn’t mean they are right.
*   Most criticism is only an opinion based on an expectation.

If you can internalize these, you will be much more receptive to hearing that your code has a flaw.

### Break the Spec into Tasks

One thing I’ve started doing recently that I’ve found to be a great help is that I’ve started breaking the specification I’ve been given down into the composite task that I’ll need to implement the specification.  How granular.  I aim for task that should take less than four hours.  By getting this granular, I’m able to accurately estimate how long it should take me to complete the specification, and I’m sure I’ve caught all of the tasks involved in completing the specification.

You should track your time against your estimates so you can get a sense of how far off your gut is relative to reality.  This will improve your ability to estimate projects.

By breaking down the project like this, you are more likely to see holes in the requirements before you even start coding.

### Create a Test Plan

The other thing I’ve started doing is that I’ve started writing out how I plan to test the specification once I’ve completed it.  I just write this out.  Once again, this helps me find holes in the requirement.  But this also forces me to start thinking of ways someone might use the code that would break it.  And that simple act of trying to break it in my mind prior to coding it, refines the spec, and makes my code more reliable.

### Ask for a Review

Once you have your tasks and your test plan, ask the person who gave you the spec to review it.  “Does this look like it reflects what you’ve asked me to do?”  This does two things.  First, and most importantly, it ensures you understand what it is you are building.  But, it also enlists someone else in the responsibility of ensuring what you finally build is what should have been built.

What QA Can Do
--------------

### Don’t Call Them Bugs

I remember reading a Louis L'amour book one where the basic plot was this wagon train going out west.  At the beginning of the trip they had all agreed that no “bad language” was allowed.  And then one day, someone used the word, “shit” to describe cow poop that was on the ground.  The group was in shock and he was reprimanded.  At that point I remember the line, “If a word makes it any different, why don’t we just call it pudding?” But you see, as I’ve already explained, a word DOES make a difference.

The word I would prefer to use is “Specification Refinement” because, in the end, that is what they are.

### Don’t Write Requirements

One thing I’ve noticed happens quite frequently is that once QA has verified all of the items in the requirement, they start doing exploratory testing, as they should.  But, when they find something, they inadvertently start writing requirements.  It looks like this.

“I did X, Y and Z.  I expected to get result 1 but instead got result 2.” Some of you are probably thinking, “What’s wrong with this?!” Well, why did you EXPECT to get result 1?  If the expectation was not listed in the requirement, you have no valid reason to expect anything.  Your expectation is just your opinion about what should happen based on previous experience.

So, how to write up this problem instead? “I did X, Y and Z and 2 happened.  This doesn’t look right but I don’t see anything in the spec that says what should happen.”

### Don’t Assign Bugs

This one is going to fly in the face of QA teams everywhere.  But remember, we are trying to find peace in what has become an antagonistic relationship.

Remember how I said that programmers react emotionally to the fact that you found a bug?  Well, if you assign a bug to them and they get a notification about that bug in the middle of writing code for the current sprint, here is what is going to happen.  First, the email is going to interrupt them.  Second, they will have an emotional response to the bug report that could continue to derail them for the rest of the day.

Instead, you should be assigning the bug to the project.  Assuming you are using Scrum and have a backlog, the issue should be put on the backlog for grooming.  Grooming would include figuring out who is responsible for the bug or who is responsible for finding out what the core issue is so we can assign the bug appropriately.

One of the problems I’ve seen with assigning bugs to specific developers is that the bug is often assigned incorrectly.

By assigning the bugs to the back log as specification refinements, they just become additional features and the sting associated with “Bugs” goes away.

If you are using software that requires you to assign bugs to an individual, make that individual the Scrum Master, Product Owner, or Project Manager (if you aren’t doing Scrum).

Organizational Changes
----------------------

Finally, I want to address organizational changes that you may need to make.  Hopefully, you are already doing this.  But my experience tells me otherwise.

### Silos Kill

Everywhere I go, QA is a separate department.  Why can’t QA be co-located with the developers?  Wouldn’t it make a lot more sense to have QA working with the developers to figure out a test plan so we can code for the plan rather than having the plan developed in isolation?  I get that exploratory testing might reveal additional issues, but certainly some of those issues can be revealed early by defining how the code is going to be explored.  Plus, making everyone part of the same team means they are all working toward the same goal.  No one gets offended that way.

When one QA person found out I was writing unit tests, she asked, “What will be left for me to tests?”  Which I found to be an incredibly naïve way of thinking.  Wouldn’t you hope that you don’t find any problems with the code I am working on?  How is the fact that I’m testing a problem for QA?  Aren’t we all working on the same goal?

### Central Source of Truth

Another place that needs to be addressed related to silos – Another area that re-enforces and is a result of silos – is this habit of each group using their own project management software.

In one organization I’ve worked at we used four different systems.  One system for version control (GitHub) another system for QA (HP Quality Center) a third system to manage requirements (which we only used minimally and instead had documents on a shared drive) and a forth system for managing our Kanban board (Jira).

The frustrating thing is that 80% of what everyone needed to do could have been achieved by using GitHub.  But even if we needed to use separate systems for the actual artifacts, it seems to me that we could use one system for tracking the project instead of having it tracked three or four different ways.  That’s just craziness.
