---
title: Automated Web Application Functional Testing
tags:
  - application testing
  - dependency injection
url: 2460.html
id: 2460
categories:
  - TDD
  - Testing
date: 2014-04-01 13:01:00
---

![WebTestingCloud](/uploads/2014/03/WebTestingCloud.png "WebTestingCloud")

One problem we often have when performing application test is that if we are testing  applications that modify the database in some way, we can’t test without modifying the database.

In an ideal world, one way you could deal with this issue is to create a test database that has known data.  But even then, you have to go through the effort of setting up the data prior to each test.

To be clear, I’m not talking about unit test.  You should have unit test for most of your code.  I’ll handle what not to test in some other post.  But, at some point you will want to put together a suite of test that ensures that the application, as a whole, does what you expect it to do.

So, the stated problem is this, “How do  we test a web application in such a way that we are always working with known data and in such a way that we do not ever modify the data in the database?”

So far, I’ve not found a way that test the whole web application from the user interface down to the database.  But, I think I’ve found the next best thing.  Split the application in half between the view and the database and perform two, overlapping, integration test.  If both work, we can reasonably assume that the whole application works.

So how would we go about doing this?
------------------------------------

If you’ve designed your application correctly, you should have a seam somewhere in the center.  In my code it tends to either be at the business logic layer or at the data access layer.  What we want to do is to create a separate class, or set of classes that look just like the classes at that layer and swap them in while testing the top half of our application and use the real classes when testing the bottom half of the application and when using the application in production.

We do this using dependency injection using your favorite dependency injection framework.  I’m not going to go into the details of dependency injection here.  Maybe some other day.  But this is a place where you would use it.

Test the top half
-----------------

In the fake DAL or BLL classes, what you are going to do is return what amounts to hard coded values.  In a recent implementation, I stored the JSON representation of the values in a resource (RESX) file  using the parameters that were passed into the method call as keys so that I could retrieve that data. If you need to do an update, you will need to store that some place so that your test code can verify that it got passed down to the function that should have saved it to the database.

By doing this, you can verify that your code from the presentation layer down to this seam works as it should.  Now we need to test the bottom half of our code.

Test the bottom half
--------------------

This will be a bit easier.  What we do here is we have our test code run the code directly.  But, unlike when we run the code in production, we will wrap the code in transaction tracking starting with our setup method.  We turn on transaction tracking, get the database into the state that we need for testing, run our test, and then rollback the changes.  Assuming the transaction tracking works correctly, and we have to assume that our third party tools do, this test everything without modifying the database.

Because we’ve tested both the top half of the code and the bottom half of the code, we now have a reasonable assurance that the code as a whole works as it should.
