---
title: Why Johnny Can't do Test Driven Development
tags:
  - bdd
  - tdd
  - test driven development
  - testing
url: 2971.html
id: 2971
categories:
  - TDD
  - Testing
date: 2015-03-05 07:00:00
---

![ppl-kid-05](/uploads/2015/02/ppl-kid-05.jpg "ppl-kid-05")

Last week we looked at a few excuses developers give for not testing their code as they develop it ([Excuses For Not Testing](/excuses-for-not-testing/)).  We finished that by mentioning that most of the code you write simply isn’t testable.  You can't practice Test Driven Development on something that isn't testable in the first place. And there, folks, is why Johnny can’t test.

<!-- more -->

But, it’s not Johnny’s fault.
-----------------------------

Think about what makes code testable.  At it’s core, testable code is loosely coupled.  But what do we mean by “loosely coupled”? Well, let’s start with the large picture.  Assuming you have a multi-layered architecture.  That is, you have your code broken out into View, Business Rules, and Data Access.  Raise your hand if your business rules access your view directly.  Would you be able to test your business rules without your view? At a finer level of detail.  How much of your code creates the objects it needs within the same class, or worse, the same method, that will need it?  Without using a Dependency Injection framework, could you swap out any objects your class uses?  Have you even heard the rule, “Classes should either create things or do things, but never both within the same class?”  If you did that, how much more testable would your code be? If you were to write a test for a method, how much setup work would you have to do?  If it is more than a few lines, your method is probably doing too much, either directly or indirectly.  You’ll need to find a way to make it do less.

Maybe in future post, I’ll address some of these issues in code.  But for now, I just want to address the problem.

Again, it isn’t Johnny’s fault that he doesn’t know this stuff.  Think about the code samples we tend to look at.

When is the last time you saw sample code that was testable?
------------------------------------------------------------

Short rant here, but I’ve been working with EXTjs (version 4.x) for the last year and a half.  Sencha will tell you that this uses a MVC architecture.  But what they mean by “Controller” really functions more like a “ViewController.”  That is, the controller is tightly bound to the view that it handles events for.  The way they have things setup, you access View elements by getters that are automatically generated for you in the view.

The problem with this is that you can’t really test the controller logic without bringing along the view.

Sencha isn’t the only company who does this.  Most of the sample code for WebForms did the same kind of thing.

Now, the reason this is an issue is that sample code is how most of the newer programmers are learning how to program.

I heard recently that statistically, because of the growth of the industry, half of the programmers available have 5 years or less of experience.  I don’t know about you, but the first 5 years of my programming career, I was still figuring out how to program.  I wasn’t thinking about architecture issues and I certainly wasn’t thinking about formal testing.  From what I’ve seen of the new recruits, I don’t think they are either.  Shoot.  Some of the ones I’ve interacted with couldn’t code themselves out of a paper bag without help.

And so, what’s the conclusion to all of this?
---------------------------------------------

I don’t know.  Maybe the first step is to admit that we have an issue here and that the issue is so much a management or time issue as it is an education and laziness issue.  That the code we generate shouldn’t assume that people will take the code and adapt it into testable code, but that we should write testable code as our sample code.  Maybe colleges should teach basic software architecture and TDD as part of the curriculum.  Maybe those of us who know better should just start testing and figure this all out well enough to explain it to others.
