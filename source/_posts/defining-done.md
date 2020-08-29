---
title: Defining “Done”
tags:
  - definition of done
  - DoD
  - programming
url: 2568.html
id: 2568
categories:
  - Programming Best Practice
  - Scrum
date: 2014-07-10 13:00:00
---

A couple of weeks ago, I mention [“definition of done”](/are-we-there-yet/) which many of my readers may have never heard of before. The phrase, “definition of done” comes out of the agile movement.  But there is no reason why it needs to stay there.  In fact, I would argue that many of the problems we have in the software industry are because most organizations only have one definition of done, “If we ship this today, can we make money?” When the Agile people talk about “definition of done” what they ultimately mean is, “if we were to ship this product today,and someone were to inspect what we’ve done, would we be embarrassed?” Definition of done, is about the quality of the code. When thinking about the definition of done, here are some items you might consider.

<!-- more -->

#### Does the code meet the requirements of the user story?

This is the most obvious.  Of course for this to work, you have to have a user story that is specific enough for you to answer this question.

#### Has all the documentation that the organization requires been updated?

This one can fall through the cracks easily because documentation is the least favorite activity of a programmer.  But, it isn’t necessarily a programmer activity.  Remember, an ideal team has all the skills it needs.  So, if you have a documentation requirement, be it an ISO requirement or simple end user documentation telling them how to use the software, your team should have someone on it that can produce this documentation.

#### Does the code have a reasonable level of unit tests?

I say “reasonable” here because the principle is high code coverage where we need it.  To try to aim for some metric will cause us to write tests where we don’t need them.  By combining a “reasonableness” level with code reviews, I think we can hit this target without setting an unrealistic arbitrary limit.

#### Do all of the unit tests succeed?

You’d think this one would be obvious.  You have tests, you should be running them every time the code changes.  But, I’ve seen situations where tests sit in version control and never get run. Run your tests!

#### Is the code covered by system level tests?

Once again, this one should be obvious.  Just because you have unit tests, doesn’t mean the system works.  The main problem with the Federal Health Care web site that went live in the United States recently is because no one made sure all the parts worked together. And don’t leave this to manual testing.  There are many ways of testing at this level that you can automate.

#### Do all the system tests succeed?

Once again, run your tests.

#### Has the code been reviewed by one other programmer?

If there isn’t at least one other programmer on your team that understands what you’ve done, you aren’t done.  I could, and probably will, write a whole post about this sometime.

#### Have coding conventions been observed throughout the code?

There are a number of tools out there  that check for coding conventions.  For CSharp, I like ReSharper.  JsHint is what I prefer for JavaScript.  There is  FxCop built into Visual Studio. Pick a standard, find a way to automatically verify the code meets the standard, and make sure nothing gets put into version control that doesn’t meet the standard!

#### Has the code passed some level of complexity threshold?

I’ve talked about [code complexity](/avoiding-code-complexity/) before.  Just go read that post.

#### Is there any known code duplication?

Again, there are tools for this.  Find one and use it.

#### When you compile, are there warnings?

[Compiling without warnings](/treat-warnings-as-errors/) is something else I’ve already talked about.  

Other Places Talking About “Definition of Done”
-----------------------------------------------

*   [What Is Definition of Done (Scrum Alliance)](//www.scrumalliance.org/community/articles/2008/september/what-is-definition-of-done-(dod))
*   [Clarifying Definition of Done (Mountain Goat Software)](//www.mountaingoatsoftware.com/blog/clarifying-the-relationship-between-definition-of-done-and-conditions-of-sa)
*   [8 Steps To Definition of Done in JIRA (Atlassian Blog)](//blogs.atlassian.com/2013/10/8-steps-to-a-definition-of-done-in-jira/)
*   [Definition of Done Creation (Mitch Lacey & Associates)](//www.mitchlacey.com/intro-to-agile/scrum/definition-of-done)
