---
title: TDD Isn’t All About Testing
tags:
  - programming
  - tdd
  - test driven development
url: 2455.html
id: 2455
categories:
  - TDD
  - Testing
date: 2014-03-25 13:07:00
---

![tran-land-045](/uploads/2014/03/tran-land-045.jpg "tran-land-045")While the artifact of Test Driven Development is test code, what you get out of test driven development far exceeds the test themselves. 

Maintainable Code
-----------------

By writing test first, you tend to write code that is more highly maintainable than if you just wrote the code to solve the problem.  By writing a class so that it can be used in both the system you are writing it for and so that it can be tested, you’ve been forced to think about the code in at least one other way from what you would have initially.  The result is your code tends to be more structured than it would have been otherwise.

Clear Specifications
--------------------

By writing test first, you are forced to develop clearer specifications.  I’ve run into this recently on a project that I’m currently working on.  I can’t write a test for the code I’m about to implement because I don’t clearly understand how this is supposed to interact with the rest of the application.  Until I do, I really can’t move forward.  If I were not writing a test, this would not be as clear now as it is.  Although, one could argue that eventually it would become clear.  But it is more likely I would leave the feature out completely because I forgot about it entirely.  Something I’ve been known to do in the past.

Up To Date Specifications
-------------------------

This leads to another advantage to test driven development that [I’ve mentioned before](/test-driven-specifications/).  By writing test in advance, you have the specification coded.  This forces you to keep the specification up to date if it changes because your test won’t run unless you do.  How many other programming methods are there that force the specifications to be kept up to date?  I can’t think of any.

A Safety Net For Refactoring
----------------------------

If you’ve ever looked at bad code and thought, “I bet I could make this better.”  But then you were afraid to make any changes because you aren’t sure your improvement wouldn’t break something, you’ll really appreciate TDD.  If there is a good test suite for the code you want to refactor, you can be sure that any changes you make won’t break something it should do.  I’ve left a lot of code alone for fear of breaking something else.

Preventing Feature Creep
------------------------

Another thing TDD does is that it prevents feature creep on the part of developers.  Face it, how many times have you added a feature into the system that no one asked for?  By coding to the test, you reduce this urge.

Many people start TDD by writing test after the fact and wonder how this can possibly be helpful.  This is because they’ve written them after they’ve written the code and they’ve completely bypass 80% of the benefits.
