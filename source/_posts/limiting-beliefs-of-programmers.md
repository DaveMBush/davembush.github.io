---
title: Limiting Beliefs of Programmers
tags:
  - agile
  - limiting beliefs
  - programming
  - scrum
  - tdd
url: 3031.html
id: 3031
categories:
  - Programming Best Practice
date: 2015-04-09 06:00:00
---

![scream](/uploads/2015/03/ppl-men-026.jpg "scream")

At the risk of making half of my audience think I’ve gone off the deep end, I’m going to address a topic that I’ve only recently REALLY begun to understand, in part thanks to [Soft Skills](/softSkills).

When I’ve heard the topic of “Limiting Beliefs” come up, it has almost always been in the context of something along the lines of “What the mind can conceive and believe, it can achieve.”  Which is easy to disprove.  At least it is out of context!  I mean really, if I can conceive and believe myself to be a butterfly, it just isn’t going to happen! However, the opposite is pretty easy to both accept and believe.  And that’s what I want to talk about today.  But even then, it probably isn’t what you are expecting.

<!-- more -->

Typically, when people talk about "Limiting Beliefs", they are talking about patterns and practices you picked up as a kid that are holding you back now.  And while those may be areas that you need to work on, what I want to talk about today is more micro than that, although they may have roots in our past for various reasons, the Limiting Beliefs I want to talk about today are common across nearly every programmer I talk to.  If you think “Limiting Beliefs” mean something along the lines of, “you are limited because you don’t believe enough,” that is NOT what I have in mind here at all.  In fact, “Limiting Beliefs” are beliefs that we believe TOO strongly and because we hold them too strongly, they limit us.  This is what I mean when I talk about “Limiting Beliefs” even if it is used in another way by someone else.

Here are some examples specifically related to the craft of programming.

You Can’t Practice TDD
----------------------

As I’ve mentioned in other articles, most programmers I know don’t practice Test Driven Development because they believe they don’t have permission.  And when they ask, they don’t get permission because you’ve transferred the belief that you don’t think it is important.

If instead, you believed you had permission to do whatever it took to legitimately do your job well, you would learn how to do everything it took to practice TDD well.

Can’t Create Branches in Version Control
----------------------------------------

I recently ran into a comment on a blog that mention this.  In fact I’ve run into this very issue where I am currently working.  But instead of thinking about what I can’t do, or when I am frustrated by what I can’t do, I first think 1) is this important enough to find a way around? And 2) what CAN I do? In this case, I found it critically important.  My productivity was being hampered because I am adding new features to existing code and, while that code is being tested, I’m converting the code to the last version of the library that we use.  Branching allows me to switch between the two projects easily and it allows me to migrate the current code into the upgrade code so that when the conversion is done, I can merge it down into the main branch and keep going.

How did I do this?  We use TFS with the old TFVS version control system instead of the Git repository.  But there are at least two projects that exist that allow you to create a bridge between your local code that uses a local GIT repository and the remote TFS repository.  There is at least one project available for subversion that allows you to do the same sort of thing.

I get my branches and yet I have not caused any disruption to the rest of my team.

Can’t Use X Technology
----------------------

OK.  So, again, ask yourself the same questions.  Is this hampering your productivity?  What CAN you do?

You Have to be Perfect
----------------------

I think many of us realize that we can’t be perfect and yet, how do you react when someone finds a bug in your code? I saw a tweet last week that captures the essence of the bug fixing process.

1.  That can’t happen
2.  That doesn’t happen on my machine.
3.  That shouldn’t happen.
4.  Why does that happen?
5.  Oh, I see.
6.  How did that ever work?

The question I need to ask is, why do we start with “That can’t happen” unless we feel that we need to be perfect.

In the last year, I’ve finally gotten to the point where my first reaction is, “OK, well, put it in the issue tracker.” (Or if you are doing Agile, “put it in the backlog”).

Can’t Practice Agile/Scrum
--------------------------

Speaking of Agile/Scrum.  Are you working at a place that doesn’t practice Agile or Scrum, but you think they should?  What parts of Agile/Scrum can you implement in the sphere of influence you have?  So, you can’t form a Scrum team.  But, do you personally put people over processes?  Do you put people first at all?  Most of the [Agile Manifesto](//agilemanifesto.org/) can be implemented at a personal level once you understand what it is really about.  Don’t expect anyone to adopt agile in your organization if they don’t see it in you first.

You Are an Introvert
--------------------

How many of us hide behind this one?  We don’t want to deal with people.  We really don’t value people over much of anything.  In fact, we think, “programming would be a great job if it weren’t for the clients.”  I’m reading some books that talk about the impact of confusing behavior with who we are.  OK, so sure, your behavior is that you prefer to avoid loud noises.  You’d rather talk to one or two people at a time.  You process stuff in your head instead of with your mouth.  That’s behavior.  To say, “I am an Introvert” can have the effect of saying, “I hate people” and can become a limiting belief because it will isolate you from the very people you should be helping.  Sorry, you can’t get far as a programmer if you avoid the people part of it.

Restrictions
------------

And then there is the general set of restrictions that come with being part of any organization.  We have a few where I work that, on the face of it, seem ridiculous.  I HAVE to take a lunch break even though my lunch consist of 5 sausage links that can be consumed in about 5 minutes.  I can’t start before 7am.  I can’t leave until 3:30.

But what can I do?  Well, no one said WHEN I had to take the lunch break, so I come in and watch a half hour of [PluralSight](/pluralSight) courses every morning.

I can’t start before 7am.  But I CAN enter the office before then.  I prefer to come earlier because the traffic is lighter if I come in at 6:45.  So, I come in and don’t start working until seven.

To get my eight hours in, I can’t leave until 3:30 anyhow.  So that is not an issue.  And if I need to leave early occasionally, no one said I couldn’t do that.

The Key To Eliminating Limiting Beliefs
---------------------------------------

Do you see what I’ve done here?  At every point where I’ve been told or believed I could not do something, I’ve change the question from “What can’t I do?” to “What can I do?”  Can’t locks you down.  It locks you out.  Can frees you. So, what limiting beliefs do you have and how can you overcome them?
