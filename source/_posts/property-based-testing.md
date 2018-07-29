---
title: Property Based Testing Revealed - A Better Way to Test
tags:
  - property based testing
  - tdd
  - testing
url: 4554.html
id: 4554
comments: false
categories:
  - Testing
date: 2018-01-23 06:30:24
---

Over the last couple of weeks, I've been experimenting with Property Based Testing.  While I'm probably doing it "wrong" by many definitions, I'm finding it useful enough that I'm adding it to my testing toolbox. <figure>![](/uploads/2018/01/2018-01-23.jpg "Property Based Testing Revealed - A Better Way to Test")<figcaption>Photo credit: [abraham.williams](//visualhunt.com/author/0651d3) on [VisualHunt.com](//visualhunt.com/re/284007) / [ CC BY-SA](//creativecommons.org/licenses/by-sa/2.0/)</figcaption></figure>

<!-- more --> 

What's A Property?
------------------

As you introduce yourself to Property Based Testing, one of the first things you'll need to understand is what they mean by a "property."  Most of us who have been doing some form of Object Oriented Programming think of "properties" as data points on an object.  That's not even close to what the word means when we talk about Property Based Testing. 

Roughly translated, a "property" is some feature of the code under tests.  Most of the beginner literature uses the classic add(a, b) method.  Properties of an add method include:

*   `add(a, b) === add(b, a)`
*   if `a` is zero then `add()` will return the value of `b`
*   `add(a, b) - a` will equal b (and similar for minus b equals a).

The key here is that we shouldn't need to know too much about the data we are sending in to our add method because we are going to send in random data.  So, any property we test needs to be expressed in a data agnostic way.

What are the Advantages?
------------------------

And while having to think about our test in a data agnostic way is hard, at first, the first advantage is that we start thinking about testing our code in terms of generalities instead of specifics.  And, really, a general test is a much more robust test. 

But, we need data for our tests, right?  Where does that come from?  Depending on the framework you are using, this may vary, but the general idea is that you describe what the data should look like, and the framework will generate random parameters to pass in.  This fact leads us to two more benefits of Property Based Testing. 

First, since all I need to do is describe what the data should look like, I'm not forced to think of the data I want to use to tests my function.  I don't know about you, but my tendency is to create data that works with my function instead of trying to create data that will break my function.  By having the framework create random data, I'm more likely to test with data that will break my code. 

And that leads to the second benefit of random data.  I'm more likely to find the edge cases for my function under test. Now, one detail I've left out is that each test you create gets run multiple times with different random data.  So, it isn't like one day your tests work great and another day they fail.  No.  The system I use generates 100 different sets of data per tests.  You can have it create more or less tests as needed.

Places You Might Not Use Property Based Testing
-----------------------------------------------

Now, running 100 different permutations on a test that takes a long time to run may not be practical.  My first question would be, why does your code take that long?  Are you trying to unit test HTML code and the rendering is taking a long time?  That's probably a poor use case. 

Another place where you might want to avoid property based tests is when you've already basically tested the component parts and you just need to do a sanity check in the integration of the parts.

Places You Might Use Property Based Testing Anyhow
--------------------------------------------------

On the other hand, not having to come up with my own parameter values makes the whole Property Based Testing pretty attractive, even when it doesn't make a lot of sense.  And it might encourage you to write smaller functions.

Problems I've Run Into
----------------------

I would say the biggest problem I've run into using Property Based Testing is learning how to write tests that don't re-implement the logic in my code in order to test the logic in my code. 

Going back to the `add(a, b)` example above.  It is tempting to test that function by verifying that `add(a, b) === a + b`.  But that wouldn't be a very good test because we used the same logic that is probably already being used inside the `add()` function.  In example based testing (classic unit testing), we have the same problem, we just don't use code to implement it.  We add the numbers in our head and verify that `add(a, b) = c`.  But, `c` is just `a + b` done in our head. 

Which leads me back to, even though it is "harder" we end up with better tests.

I'm Probably Doing it Wrong
---------------------------

I'm so new to this way of thinking, that I'm pretty sure I'm doing everything wrong.  But, I figure I can take advantage of what I understand and as I write more tests, I'll find more and better ways of writing tests.  I encourage you to pick up a property based framework for your development environment and give it a try.

Resources:
----------

*   [What is Property Based Testing?](//hypothesis.works/articles/what-is-property-based-testing/)
*   [An Introduction to Property Based Testing](//fsharpforfunandprofit.com/posts/property-based-testing/)
