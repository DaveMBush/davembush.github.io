---
title: The Proper Function of QA
tags:
  - QA
  - Quality
  - roles
url: 2487.html
id: 2487
categories:
  - TDD
  - Testing
date: 2014-05-01 13:18:00
---

![QA](/uploads/2014/04/QA.png "QA") With all this talk of test driven development, one has to naturally ask, “Where does the QA department fit in?  Do they even have a role in the organization any more?” Well, the answer to that is, “absolutely but probably not the role they think they have.”

<!-- more -->

QA People Are Not Programmers
-----------------------------

Once again, as I visit organizations, I find that they are trying to use automated testing tools to script their test.  This ends up being a fool’s errand because most QA people are not programmers and to create a script that is actually going to work reliably, that is going to take a programmer.

Jr Programmers Are Not QA People
--------------------------------

The other related problem I frequently see is that organizations tend to put junior programmers in the QA department to handle this “trivial programming task” when what they really ought to be doing is placing senior level programmers in the department because they are more likely to be able to predict what could go wrong.  But I digress.

By moving to a test driven development process, you actually end up solving the problem of QA needing programmers because you’ve moved the creation of the scripts away from the QA department and back on to the programming staff where it belongs.

So what is the point of having a QA department if they are no longer going to create test scripts?

The Proper Role of QA
---------------------

They have the same function they’ve always had.  Their job is to try to break the program.  To find all of the conditions the programmers left out when they wrote the code and to report those problems back to the programming staff where they can write a test for the problem and fix the bug.

This may sound like a job that anyone can do, but remember that once they’ve broken the program they have to document how they broke it in a way they can repeat so that when the problem is reported to the programming staff, the error is easy to find.  This requires an attention to detail that few people have.
