---
title: 100% Code Coverage Possible?
tags:
  - code coverage
  - tdd
  - unit test
url: 2987.html
id: 2987
categories:
  - TDD
date: 2015-03-26 06:00:00
---

![100% code coverage](/uploads/2015/03/iStock_000005626017Medium1.jpg "100% Code Coverage")

In response to my post “[Excuses For Not Testing](/excuses-for-not-testing/)” [Kris K](//disqus.com/by/mayofcherries/) asked:

> There is also another side of [Unit Tests](//en.wikipedia.org/wiki/Unit_testing "Unit testing"). Some companies are so fixated they aspire to have 100% Unit Tests coverage and they make programmers write Unit Tests for legacy code for no reason. Just for the sake of having Unit Tests. … \[I\] wonder if you had any similar experiences and what you think about this approach. I guess the 100% extreme is better than no tests at all, but it can make the developers very bored and feeling useless.

And my initial reaction to this was, “WOW!  So much to respond to here.  I think this is worth a blog post.

Is 100% realistic?
------------------

So, first, the question of 100% coverage.  Is that a realistic goal?  And the answer of course is, “it depends.”

#### The first dependency is, of course, “what do you mean by ‘100% [code coverage](//en.wikipedia.org/wiki/Code_coverage "Code coverage")’?”

Do you mean you need tests for not just all of the code you wrote, but all of the code your tools wrote for you and all of your third party libraries?  If 100% is inclusive, I’d lean toward 100% not only being not realistic, but also not necessary. Although, in a highly critical system, you’ll probably want to do some kind of vetting of the tools and libraries you are using.  But by the time you are using them, you should already know that they work, the only thing you won’t know for sure is if you are using them correctly.  In this case, you are looking at integration tests and not unit tests.

#### The second dependency is, “Is the code testable?”

This is a little trickier to evaluate, so let me illustrate.  There is code that isn’t testable that should be.  I’m not talking about that code.  I’m talking about code that just can’t be tested. Let me illustrate. I’m working with [EXTjs](//www.sencha.com/products/extjs/ "Ext JS") which uses their proprietary implementation of MVC.  The part that is particularly interesting is that the View part of the architecture is basically a JSON object that defines what the view looks like.  While you can put in code that performs some action, the fact of the matter is that this code, when coded correctly, doesn’t really DO anything.  So, I would ask, how would you test this?  Sure I could put it in a test harness and run it through  some sort of parse routine or something, but unless you can say, “when I do this, this other thing over here should be true (or false)” you really can’t test the code.  A class that has no methods is basically a structure and structures can’t, by definition, be tested. On the other hand, I wrote some code once in WinForms that I was able to get under test and execute by calling the methods directly in the form.  I would have been better off moving that code into a presenter class so I could test the functionality separate from the presentation (view) and marked the view as untestable.

#### Finally, I have to ask, “Is refactoring allowed?”

This is legacy code we are talking about.  And legacy code is notoriously hard to unit test.  As I mentioned a [couple of weeks ago](/why-johnny-cant-do-test-driven-development/), in order to write unit test, the code has to be testable in the first place.  So, if the code isn’t in a condition that it can be unit tested, and you aren’t allowed to make the code testable, then either 100% code coverage isn’t possible or you aren’t unit testing.  It’s that simple.

Testing is boring
-----------------

Next, I’d like to address the issue of testing being boring.  I can understand this.  Most of my day is spent unit testing code I’ve previously written because when I wrote it, I wasn’t as good at unit testing as I am today. But, you know what I’ve found along the way?  That by making sure the code is covered 100% it has made me think about how I’ve structured my code and it has made my code more maintainable. But, mostly I find testing entertaining because I learn just a little more about testing.  I get to prove to myself and my peers that writing unit test really does find bugs we didn’t have any idea were there.  I get to perfect my craft just a little bit more. Here are some things to consider about the statement that “testing is boring.”  You may not like what I have to say, but keep in mind, I don’t know you and I don’t know Kris.  I just know what the research says about the state of being bored.

#### [Dictionary Definition](//www.macmillandictionary.com/us/dictionary/american/bored)

> feeling impatient or dissatisfied, because you are not interested in something or because you have nothing to do.

#### [People who get bored are anxious people](//www.dragosroua.com/how-and-why-we-get-bored/)

> People who get bored easily are usually anxious people. They’re also having quite a low level of self-esteem. If you’re constantly challenging yourself by trying to stop what you’re doing, because you don’t “like” it, you end up considering yourself an inappropriate person.

#### One Of My Professors Said

> Boring people get bored

  Personally, I find that I’m most bored when I have something else I’d rather be doing.  It isn’t so much that I don’t want to do what I’m currently doing as I’d rather be doing something that I can’t currently do.  I had this experience on Friday because on Thursday night I had found an article on SEO stuff that I desperately wanted to start putting into practice. What I’m suggesting here is that if you change your perspective from one of, “this is a ridiculous idea” to “this is a challenge to be met” or “this is an opportunity to learn”, or even (to address a specific comment that started this post) “I get to make my this application more bug free.”  you may find that the boredom leaves. Treat it as a game.  Maybe pair up with people on your team and see who can discover the most bugs during the day.  Create a leader board.  Since you have to do this anyhow, make it fun.

##### Other places talking about 100% code coverage

*   [The Fallacy of 100% Code Coverage](//binstock.blogspot.com/2007/11/fallacy-of-100-code-coverage.html)
    
*   [How to Fail With 100% Test Coverage](//jasonrudolph.com/blog/testing-anti-patterns-how-to-fail-with-100-test-coverage/ "Testing Anti-Patterns: How to Fail With 100% Test Coverage [jasonrudolph.com]")
