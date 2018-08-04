---
title: Humpty Dumpty and Programming
tags:
  - agile
  - design patterns
  - programming
url: 4518.html
id: 4518
comments: false
categories:
  - none
  - Opinion
date: 2017-12-05 06:30:32
---

I've noticed a pattern in the programming world at large both with programmers and with managers.  We define things how we want them to be for our organization and not how they are.  We are like Humpty Dumpty who says, "When I use a word ... it means just what I choose it to mean -- neither more nor less." 

There are two places where I see this pattern manifesting.  The Agile movement and Design Patterns. <figure>![](/uploads/2017/12/2017-12-05.png "Humpty Dumpty and Programming") Photo by [aturkus](//visualhunt.com/author/f31767) on [Visualhunt](//visualhunt.com/re/b4881b) / [ CC BY](//creativecommons.org/licenses/by/2.0/)</figure>

<!-- more --> 

Agile
-----

Longtime readers are familiar with my rants against the failure of Agile.  Ever job interview I go to eventually ends up asking the same question. 

"Have you ever worked in an Agile organization before?" 

I have a lot of issues with this question, but my answer is always the same.  "I've worked in several organizations that call themselves Agile, but I've yet to work in one that really is."  And there is the problem.  Even if I say I've worked in an Agile organization, there is no possible way you can be sure I've worked in an Agile organization that defines Agile the way you describe Agile.  So, why even ask the question? 

It is like Humpty Dumpty trying to explain the meaning of the poem [Jabberwocky](//www.jabberwocky.com/carroll/jabber/jabberwocky.html). We define Agile with the bits we like and ignore the bits we don't like or don't understand, like the 4 blind men "looking" at an elephant and then wonder why it doesn't really work for our organization. 

Next time someone ask me that question, I may just answer the question with another question, "Why do you ask?" or "How do you define 'Agile'?" 

Seriously! What's the point of asking the question when it doesn't tell you anything about the applicant you are interviewing?  Agile has become such a major buzzword that I doubt you'll find any applicants that haven't worked in an organization that calls itself "Agile."

Design Patterns
---------------

The more popular the design pattern, the more likely we are to see the exact same issues in our programming.  Currently, we can most clearly see this in the MV* design pattern.  Here again, people are using the design pattern based on what they imagine it to be. 

In an article I wrote several months ago, someone recently commented about MVVM, "Isn’t that the way MVVM works? views don't have business login, only pure “view” logic, the ViewModel is the one having business logic." 

This is a common misconception.  That the ViewModel, or the Controller, or the Presenter (MVP) are where our business logic go.  This completely ignores the fact that MV* is a View layer design pattern.  The View part of the MV* is the part within the larger View layer that is responsible for rendering state.

A Community of Hacks?
---------------------

Are we just a community of hacks?  I think maybe we are. All we care about is that we've shipped some code. We, largely, don't care about our craft.  If we were artist, we starve.  Not because artist starve (which is a myth by the way) but because the code we produce is so crappy, no one would consider it valuable. 

If we built houses, we'd never get past the building inspectors.  If we were architects, the houses would never get built because the plans are too confusing. 

The blessing and the curse of programming is that we can change things quickly.  Because we can change things quickly, this has us believing there is no need to be careful. 

Each year we need more and more programmers to work on code because the codebase becomes crappier each year.  No one cares.  In the 30 years I've been programming, I've only had my code reviewed as a practice in two organizations.  That alone should tell you something about the state of our code.  And for all the claims about being Agile, none have used any best practices that grew out of Extreme Programming!

What If?
--------

In previous post I've explored both sides of the technical interview process.  Up until recently, the technical interviews focused on the language, the framework, the tools.  And we try to develop an interview process that assures us that the applicant can actually use those tools.  Then when we hire them, and they can't actually code. We wonder why? 

What if we got beyond tools to how people think?

Code Puzzles
------------

Recently, I've been challenging myself with coding puzzles that are typically used at places like Google, Facebook and Amazon.  Problems that get at issues such as BigO notation, Binary Trees,  Memoization and much more.  I'm doing this for several reasons.  First, working on problems like this reminds me that I'm really not all that smart.  Oh, I can get by, but I don't challenge myself to produce the best code possible.  Maybe the rant above is more about me than the industry, but I don't think so.  I think I've risen (or more accurately, sunk) to the level of the people I'm surrounded by who themselves are only as high as the people they've been working with. 

I'm also doing these problems because being able to do them will inform my code.  Maybe I'll never actually need to know about depth first vs breath first searches of a binary tree, but if I can do those problems, I will have additional tools in my toolbox when I code the mundane things.  

And finally, these kinds of problems almost always have edge cases I don't see.  I really need to get better at discovering edge cases before my clients do. 

And now, here's the big question.  If these are the kind of questions that Google, Facebook, Amazon and others are using, what do they know that other companies don't?  Could it be that hiring programmers that can answer these kinds of questions not only ensures that the quality of the code is better, but is actually cheaper in the long run?  Why not hire programmers who are a dime a dozen and can get the job done, but produce crappy code in the process?  I mean, if code quality doesn't matter like most of our industry thinks, why do these successful companies not just go hire warm bodies?

The Advantage
-------------

The advantage to hiring based on how people think rather than on what tools they know is that when the tools change, it won't matter to the developer who can think through these tougher issues.  On the other hand, those who can't won't be able to grasp some of the newer concepts that show up in newer tools.  I've seen this first hand as I've tried to explain NgRX, RxJS and Functional Programming generally to some of my peers.  Are they difficult concepts.  Sure they are! Are they worth learning? Absolutely!

Be Intentional
--------------

So, what's the point of all of this?  Mostly, be intentional. Don't coast. Learn everything you can about your craft. 

Do you really know what MVC, MVVM, MVP, etc are and how they work? Or, are you just working off of what someone else has told you? 

Do you really know what Agile is? 

How many design patterns do you know that aren't the hot new trend? 

Could you code your way out of an interview with Google, Facebook or Amazon? 

Where do you want to be with your career next year?  In 5 years? 

Are you average or striving to be awesome? 

Join me on the journey!
