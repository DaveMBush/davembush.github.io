---
title: YAGNI - You Aren’t Going To Need It
tags:
  - tdd
  - xp
  - yagni
url: 2607.html
id: 2607
categories:
  - Programming Best Practice
date: 2014-08-14 13:00:00
---

One of the design principles in software development is to only write what you need today.  This has taken on the moniker of YAGNI (You Aren’t Going To Need It).

The question is, in what ways do we violate this principle?

<!-- more -->

Features:
---------

The most obvious situation where we tend to violate YAGNI is in feature creep.  We design in a new end user feature that seems really cool to us.  We may even try to sell the end user into believing they need the feature.  Maybe it is just eye candy.  “We can make this screen animate when it opens.”

Yeah, but is that a requirement or a nice to have?  How much longer will it take to design that?  How many more bugs will it introduce?

I think most of us get YAGNI at what I would call the “Visible Feature” level.  That is, what the end user will see immediately.

Where I think most of us get this wrong is at the code design and architecture level.

Architecture:
-------------

#### Do you have a tendency to over architect your code?

This is an impossible question to answer because I think all of us think we architect to just the right level.  So what are some signs that you have gone too far?

Well, one of them might be how you defend your decision to write your code in a particular way.  If your answer is anything remotely like, “We might need it to do…” you are probably over architecting.

For example, when you write your code, are you thinking about all the ways it might be used in the future?  That business rule you are writing, it might be used as a web service, or need to be accessed by JSON.  You never know.

But really, unless you need it to do all of that stuff today, you are probably spending more time on the code you are writing than you need to.  Why?  Because most of the time all of those, “we might need it to…” never happen.  And when they do happen, they happen in entirely unexpected ways that you can not foresee.

#### But it will save time!

Will it?

If your code is so complex that adding in the “it might need to…” in the future is going to take more time than adding it in now, you probably have a fundamental problem with your code.

Worse, as mentioned above, you will probably never need it anyhow.  So, you’ve spent extra time now to save time you wouldn’t spend in the future if you were to just wait until you need it.

Aren’t you busy enough with deadlines as it is?

Prevention:
-----------

So, then the next question is, “how do we prevent over architecting?”

Simple, you write Unit Test for all the code you are writing.  If you are writing code so that it can be accessed in multiple ways, you’ll need to write test that prove it can be accessed in multiple ways.  Are you willing to do that?

When the tests need to be updated to reflect new business requirements, are you going to want to update them all?

So the next time you, or someone you know says, “It might need to…” maybe you should ask them what the chances of them needing that really are and is it worth spending the extra time on it now.
