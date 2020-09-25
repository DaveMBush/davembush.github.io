---
title: What Not To Test
tags:
  - programming
  - tdd
  - test driven development
url: 2464.html
id: 2464
categories:
  - TDD
  - Testing
date: 2014-04-09 12:30:00
---

![WhatNotToTest](/uploads/2014/04/WhatNotToTest.png "WhatNotToTest")

Many people believe that implementing Test Driven Development means that you need to have a test for every line of code in your system.  When  they start thinking about TDD in this way, they start to feel overwhelmed and quit before they even start.

I know I did.

In fact, I’ve seen suggestions on places like StackOverflow that suggest as much.

But there is code in your application that you shouldn’t bother to write a test for.

<!-- more -->

Generated Code
--------------

Generated code includes any code that uses some automated process to create code your system is using.  This would include code that was written by Entity Framework, NHibernate, or code generators that you may have written.  Of course, I’m assuming that you’ve written test for the code generators that test both that they generate the expected code and the code that was generated works as expected.  But to write a test for every instance of the code the generator writes is quite a bit of overkill.

Simple Getters and Setters
--------------------------

If all your getters and setters are doing (properties) are retrieving and setting some backing store, there isn’t much point in writing a test for them.  One would assume that the code will get tested in the course of testing the code that is ultimately using the getters and setters.

Third Party  Libraries
----------------------

While I know it isn’t true, you have to assume that the third party library you are using actually works.  If you can’t assume that much, you should probably write it yourself.

Your Thinking About It Wrong
----------------------------

I would argue that if you are thinking about what you should write a test for, you are probably still thinking of Test Driven Development as something you do for the sake of testing rather than for the sake of [design, maintenance, and problem solving](/tdd-isnt-all-about-testing/).

When you write code, you should be thinking, “What problem am I trying to solve?”  Or better yet, “How can I state the problem in terms of a ‘When/Then’ statement?”

When you think about the problem this way, what you test becomes that When/Then statement.  The class name for the test becomes When and the test becomes Then

``` csharp
public class WhenTheCodeIsInStateXAndIPerformActionYOnIt
{
    [SetUp]
    public void Setup()
    {
        // Setup When
        // Perform Action
    }

    [Test]
    public void ThenItShouldEndUpWithZState()
    {
         Assert.That(object, Is.InSpecificState());
    }
}
```

When you do this, the question no longer is about how much code you have to test, but instead becomes “Have I written a test for every reasonable condition this class may encounter?”
