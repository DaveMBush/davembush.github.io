---
title: How to Sabotage Estimates
tags:
  - estimating
  - programming
  - software
url: 4017.html
id: 4017
categories:
  - Programming Best Practice
date: 2016-08-23 06:30:00
---

Over the last week, I’ve been helping other programmers estimate the task they’ve been assigned and this has caused me to reflect on how I estimate software.  What works.  What doesn’t.  What mistakes I see people make.

There has also been a move to avoid estimates entirely.  The argument goes something along the lines of, “we know the least about a project at the beginning of the project, so we can’t really give an accurate estimate.”  Which is mostly true.  And yet, there are people who need to know “how much is this going to cost?”  What do we do for them?  How do we balance the two realities? And then all of this lead me to think of all the ways we sabotage our estimates, or our estimates are sabotaged for us.

You might think that estimating projects only applies to project managers.  But the truth is, most places I have worked rely on programmers to give them estimates, and frankly, most of us screw this up.

<figure>![](/uploads/2016/08/image-1.png "How to Sabotage Estimates")<figcaption>Photo credit: [Musée McCord Museum](//www.flickr.com/photos/museemccordmuseum/2918567169/) via [Visual hunt](//visualhunt.com/photos/people/) / [No known copyright restrictions](//flickr.com/commons/usage/)</figcaption></figure>

<!-- more -->

Working Backwards from a Date
-----------------------------

This is the most obvious, I think, so we’ll get it out of the way first.  But it has some permutations that aren’t quite so obvious that we need to examine as well.

Now, I get it.  There are some cases where there is a specific end date.  Something that is out of your control is happening on a specific date.  If you don’t deliver the software by that time, then you might as well not create it.  But, in that case, you have to take the golden triangle into consideration.

*   You can have it on a specific date.
*   You can do it all.
*   You can do it within budget.

Unfortunately, even each of these have limitations.

*   You might be able to release something on a specific date, but it may not be all that was intended.
*   Even if you could buy all of the resources you needed, which is hardly ever possible, there will be limitations to using all of the resources.
*   If you have a limited budget, doing it all will probably suffer.

This is where using Scrum has a huge advantage.  Because at least using Scrum, you are constantly picking the next most valuable feature to implement and each feature is released in a potentially deliverable state.  This is quite unlike the project I’m working on where components are being worked on that we can’t even really test because they get called by something that has yet to be built.  Grant it, they’ll eventually be needed to have a viable product, but it is really hard to argue they are the NEXT most valuable feature when they can’t even be tested.

Now, if you are working backward from a date, this could work if you add more resources, or you cut features.  I’ve worked in some organizations that were really good about figuring out what could be done and making sure the client was on board with the fact that we would only deliver that much.  But there have been other organizations where every time I suggested we not deliver a feature, I’ve been given a hundred reasons why that couldn’t be done.  Needless to say, those projects are doomed.

Working backwards from a date is largely a management issue or a client issue.  But as they ask programmers for dates in light of the date, I find programmers are inclined to try to give numbers they think their managers want to hear rather than what they expect can really be done.

Going with Your Gut
-------------------

The next most dangerous way of providing an estimate is to just go with your gut feel.  Here’s the thing about your gut.  It’s pretty optimistic.

I find that everything takes about twice as long as I think it will.  I’ve worked for some people who I quickly learned were only describing half of the project.  In those cases, I’d be off by a factor of four.

So, I’ve learned to compensate.  Whenever I’m asked for a “rough estimate” I always double whatever my gut thinks.  In some cases, I’ll pad even more.

But the right thing to do would be to spend a few hours and break the project down into task that take less than a day.  Ideally you want to break those tasks down into half day sized tasks.  Once you have all of the tasks spelled out, you can add up the hours and you’ll have a pretty good idea of just how long the project will take you.

If you are working in an office, don’t forget to account for meetings, helping others, and other tasks that tend to happen over the course of a week when you provide the date you plan to be done.  Otherwise, you’ll find you’ll end up having gotten the hours right but still end up being late.

How do you break the plan down?  What I do is I ask a very obvious questions.  What’s the next thing I would need to do? Let’s assume we are starting a new feature.  If you were to sit down and code that feature right now, what is the next thing you’d have to do to code that feature?  It might be, “setup the programming environment” or “setup version control” or “get a test harness in place” or “create a branch from version control” it doesn’t matter that it might take a half an hour to do.   That’s actually good.  Just don’t let it be so big of a task that it is going to take you several days to complete.

Next, assuming you’ve done the tasks so far, what is the next thing you’ll need to do.

You just keep doing this until you have a set of task that fit within the half day sized and all fully describe the steps you will take to get the project done.  While breaking task down this way will get you a much more accurate number, I’ve found that I still need to double the time I think it will take.  Especially if I know little or nothing about the project I’m getting in to.  The more you learn about the project, the less padding you should need to do.

Don’t Plan for the Unknown
--------------------------

But in every project, there will still be some unknowns.  In fact, this past week, I hit one of those.  When I put it on my task list, I put it in as 8 hours figuring there would probably be some refactoring I’d need to do.  But when I got to it, what I found was that the refactoring project was a lot bigger than I had expected.

In this case, the best thing to do is to raise this new information to the team and adjust your estimate based on the new information.

At the end of the day, I don’t expect this will actually impact my delivery date because some of my other task are going faster than I was expecting, but I’m still adjusting my estimates to document the fact that it had to be done.

Forget Who Is Asking
--------------------

Most people I work for give me pretty accurate descriptions of the problem I’m going to work on.  I can safely double the amount of time I think it will take and I generally hit my dates.

But occasionally, there is that client who only tells you half of what he’s actually thinking.  You’ll learn who that is in your life soon enough.  You can either complain that you didn’t get complete enough requirements.  Or you can plan on the requirements being incomplete and pad your estimates accordingly.

Telling your client or manager (who is your client in most cases) that they did it wrong, may not be the most prudent way of dealing with this issue.  I opt for padding the estimates.

Working from a Summary
----------------------

This kind of gets back to the last two points, but it needs to be called out separately.

You see, most of the time you’ll be asked for an estimate with enough information to make the call.  But there are other times all you’ll have is a working summary.  I find it best in this case to give the person asking for the estimate such a wide estimate (1 – 8 hours) that it is worthless for anything other than a rough ballpark idea of how much effort the project will take and explain that I would need more information to give a better estimate.

Most people are pretty understanding when you explain this correctly.  Whatever you do, don’t give a fixed number that you can’t adjust up and down as you discover more information.  In fact, even when you have more information, you should always give your estimates in terms of ranges of time.

Assume it Will Work
-------------------

Even the seasoned programmers make this mistake.  I know one guy who brags about how quickly he can write the code.  But he completely ignores the fact that while he spent a month writing the code, he’s spent three months getting it past the QA department.  So, did he really get the project done it a month?  Oh and those bugs?  God help you if you suggest those bugs are HIS fault! But, I find we all tend to do this in some way or another.

All I want to say about this here is this.  You can either plan to test your code using some sort of test framework or you need to plan to fix bugs.  Which you plan for is up to you, but those of you who have followed my blog know I’m going to tell you that you’ll save a lot of time if you plan to write and execute tests.

Conclusion
----------

I’m sure many of you could have written a post like this.  So, I invite you to add to it in the comments below.
