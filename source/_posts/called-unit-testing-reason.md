---
title: It is called "Unit Testing" for a reason
url: 2622.html
id: 2622
categories:
  - TDD
  - Testing
date: 2014-08-28 06:00:00
tags:
---

One of the problems I had when I started to learn unit testing, and a concept that seems to be hard to grasp as I teach others about unit testing, is this concept of testing just a unit.  What is a unit after all?

<!-- more -->

What is a "Unit"?
-----------------

Simply, a unit is one feature or functionality that your application needs to perform.  The problem I see most people have is not that they don't know how to test a unit so much as they don't understand what "a feature" or "a function". This is best illustrated by a recent testing problem. The feature that was being introduced was a classic role based security issue with a twist.  A user could be one of three different roles.  But the record they are trying to view could also be part of three separate roles.  You can look at a record based on the intersection of the two. How would we test this?  More accurately, how would you design this so that you could test this? If you think of the problem as "can we log in?", you will attempt to test this as one unit.  But you actually have at least three units to this problem.

1.  What role is the logged in user
2.  What role is the record you are trying to view
3.  Given a particular user role and a particular record role, can the user view the record.

If we have three user roles, and three record roles, this means we will need three test for each of the user and record role functions and nine test for the can a user see a record.

To be able to test this properly, we will also need to ensure we have three separate methods, and possibly three separate classes in our system.

Why is testing "Units" difficult to understand?
-----------------------------------------------

But now that we've broken out the problem, we will probably still run into trouble when we start to create test for this because, if you are like most of the programmers I know, you'll make at least one of these methods call down to your data access layer in order to figure out what role who is in. Now, I understand why you might do this.  Most of our code looks something like this:

View --> Business Rules --> Data

That is, the view calls down to the business rules which calls down to the data. The problem with this way of thinking about your code is that you will always have dependencies on other code further down the chain making the code nearly impossible to 'unit' test. What you will need to do, instead, is to think about how you will test this code as you are coding.  You've heard me say it before, but this is one of the benefits of unit testing.  It makes you think about the design from two different perspectives, which ultimately makes you code more solid and more flexible. So, one way you might go about separating your code from the data is by using dependency injection.  What I'm talking about here is simple injection.  No frameworks. So, let's say you have a class you may have called user role.  Given a user id, it will return a role.  How could we code this so that it doesn't matter where the code comes from? By declaring an interface to a user role object maybe and then passing an object of that type to the constructor. By doing this, you can use a fake object when you are testing and a real object when you are using the system in production, but your code won't really care which one is being used. At some point we will need to retrieve data.  But the data is always just a side effect.  If you have a way of getting at the data, and you are confident it works, because that standard mechanism has been tested, then you don't need to write test for the data access piece, you only need to write test for "given I have good data, this method will do this."

Why Unit Test Generation Tools Are Dangerous
--------------------------------------------

Maybe you've noticed tools that will look at your class and create unit test for each of the public methods.  Maybe you think, "cool, I'll get one of those and it will tell me what to write test for!"  But if you don't understand what you are testing, these tools can leave quite a bit out of the picture.

For example, if you have a method that you need to test that has two parameters and depends on a property being set, you have all of the test that you need to write to make sure it does what it is suppose to do.  But you also have several test that you need to write when bad parameters get passed in.  Test that a tool can't stub out for you because it doesn't know anything other than that you have a method with two parameters.

This isn't to say that test generation tools aren't helpful.  But, as with most tools, they only become helpful once you understand how to test by hand.
