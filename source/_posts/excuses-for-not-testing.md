---
title: Excuses For Not Testing
tags:
  - bdd
  - tdd
  - test driven development
  - testing
url: 2940.html
id: 2940
categories:
  - TDD
  - Testing
date: 2015-02-26 07:00:00
---

![ppl-kid-044](/uploads/2015/02/ppl-kid-044.jpg "ppl-kid-044")

As I started my own journey into unit testing, I slowly began to realize that it was really easy to come up with reasons to NOT test my code as I was writing it, even once I understood what that was supposed to look like. The reason I think most programmers don't unit test code, once they understand what it is they are supposed to be doing is that they don't feel like they have permission. To this I also answer, "How much permission do you need?"

<!-- more -->

Do you really need permission?
------------------------------

Do you ask for permission to compile and link the code? Do you ask for permission to write every line of code to make the system do what it should do? Do you ask for permission to run your code periodically to make sure it does what you had in mind when you wrote the code? Do you periodically add code that makes you feel good but is not directly related to the task at hand? (Admit it, I don't think I know of any programmer who doesn't.) Then why do we feel like we need permission to write unit test for our code?

Are you convinced that you need test?
-------------------------------------

We don't write test code because we aren't convinced it is necessary to do the job we've been given. We complain that our managers don't want us to write unit test. But the problem is that you asked for permission in the first place. And, by asking for permission, you've basically told your manager that unit testing is optional. Your manager has said "no" because he thinks YOU think it is optional.

It’s not your manager’s job
---------------------------

It isn't his job to understand that not testing will produce technical debt. He's not even interested in understanding what technical debt is. All he cares about is this. When will this project be done?  When you say it is done, will it work as expected or will it have a lot of bugs that need to be fixed yet? Most of the managers I've worked for in the past will accept whatever number I give them once they understand that when I deliver the software to them, it is going to work.  In fact, I’ve even gotten asked to do jobs BECAUSE my code tends to work more often than anyone else they know who could do the job.

Now, I will admit, that in some cases there are places where you've been explicitly told to not create unit test. But even here I will assert it is because someone asked management the question.

Why?
----

And so, we need to evaluate why it is we think creating unit test are optional. Probably because what we've been doing for so long seems to be working and, when we try to incorporate unit test, the process seems slower.

And it is.

Initially, writing unit test is slower just like writing using a new language or a new framework or anything else new is slower than what we know.

But the ultimate efficiency that writing unit test as we code provides has been proven to more than offset the learning curve involved.

There is one other valid reason for not testing and that is, we simply don't know how.  This is almost as big of a reason as not believing it is worth while.  But, I think if we thought testing was really worth while, we'd start testing and figure it out as we went along.

If you think about your career, I bet there are a lot of things you know now that you didn't know when you started out.  The fact of the matter is, most of us learn on the job.  We start out with basic skills, but it is the day to day implementations that improve those skills.

> ##### Don’t let the good enough be the enemy of the perfect.

Don't let the good enough be the enemy of the perfect.  Your first set of test will be garbage.  As you stick with it, you'll wonder what you were thinking when you wrote your first test.  But this should not deter you.  This is what happened when you first started coding.  Maybe it is still happening.  No worries.  It is the practice that will make you better able to write tests and better able to write code that is testable.

And there is another reason we don't test.  Most of the code you are currently writing simply isn't testable.  But, that’s the subject for another post.
