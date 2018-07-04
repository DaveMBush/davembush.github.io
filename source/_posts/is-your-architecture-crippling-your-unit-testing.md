---
title: Is Your Architecture Crippling Your Unit Testing?
tags:
  - archietcture
  - tdd
  - unit test
url: 2629.html
id: 2629
categories:
  - TDD
  - Testing
date: 2014-09-04 06:00:00
---

Last week I wrote a post that talked about Unit Testing and the need to make sure you are only testing one particular unit of code at a time.  The post was well received.  But I am surprised that no one commented on the glaring hole I left in the post.

In that post, I said:

> So, one way you might go about separating your code from the data is by using dependency injection.  What I’m talking about here is simple injection.  No frameworks.
> 
> So, let’s say you have a class you may have called user role.  Given a user id, it will return a role.  How could we code this so that it doesn’t matter where the code comes from?
> 
> By declaring an interface to a user role object maybe and then passing an object of that type to the constructor.
> 
> By doing this, you can use a fake object when you are testing and a real object when you are using the system in production, but your code won’t really care which one is being used.
> 
> At some point we will need to retrieve data.  But the data is always just a side effect.  If you have a way of getting at the data, and you are confident it works, because that standard mechanism has been tested, then you don’t need to write test for the data access piece, you only need to write test for “given I have good data, this method will do this.”

As I reflected on the post, I realized that this may make sense to you if you are already implementing this in your code.  But, if you are new to dependency injection and the concepts of loose coupling in general, this might be a foreign concept for you.

At the very least, I realize now that it could be flushed out a bit more.

So, here we go.

N-Tiered Architectures
----------------------

There are two primary architectural patterns in the coding ecosystem that I live in.  The first, which has been around for a very long time is the N-Tiered, or 3 Tiered architecture.  The main point of this is to separate the Presentation from the Business rules from the retrieval of the data.

Code written using this model normally has the view code dependent on the business logic code which is dependent on the data retrieval code.  And, even though we can use interfaces and dependency injection code to swap out each layer, most of the code written using this architecture that I’ve seen ends up being far too dependent on the code under it than it should be.

MVC or MVP
----------

The second architectural pattern I see a lot of is the Model View Controller(MVC) pattern and it’s close cousin, Model View Presenter(MVP).  This pattern also attempts to separate the presentation, and the data and uses the controller as the way of moving the data between the two.  In this pattern, you will often see the controller or presenter layer used as the location of the business logic.  But strictly speaking that’s not the intended purpose.  When the controller or presenter is used for business logic you end up with a similar problem here that we end up with in a strict 3 tiered architecture.  The code ends up being far too dependent on either the architecture or the specific implementation of the architecture.

The Problem
-----------

Now here’s the deal.  And I think this is where many people go astray with architectural patterns.  Just because you are using 3 Tier, MVC, or MVP (or whatever) doesn’t mean that all of your code HAS to fit into the mold of being one of the following:

*   a View,
*   a Controller, Presenter or Business layer, or
*   a Data Access Layer or Model.

Think about this.  How much of the code you’ve written, were you to really switch out the view, would have to be rewritten?  How much of your code could withstand the jarring effect of changing how you accessed the data.

And I’m not talking a simple change.  From the data side we have multiple ways of accessing data in .NET.  We can use LINQ, we can use typed DataSets, we can use some ORM tool.  Say you decided to switch from whatever it is you are using now to one of the others.  Could you do that easily?

And all of this points out the main issue I see with these frameworks.  The frameworks are only there as a way of moving data around.  With each of these it is expected that we will implement the actual presentation, business logic, and data access in an architectural agnostic way.

Which finally brings us back to the comment I made last week.

The Fix
-------

All of your business logic shouldn’t really care where it got its data from.  It should not know where the data came from.  It should not request its own data.  It should be handed data in a form that can be used regardless of what the presentation layer looks like or what the data access layer looks like.

In a three tiered model, the middle layer should work like the controller or the presenter.  It should retrieve the data from either the data layer or the presentation layer and then hand that data off to some other class that will do whatever processing needs to be done, get the return value from it and then pass it on to either the view or the data access layer.  In order to make the actual processing of the data from the view or the data access layer testable, the code we write must be architecturally, and framework agnostic.

Similarly, in the presentation layer, if you have validations that you need to run on the screen, those validations need to be written in such a way that it won’t matter if or how the presentation layer changes.

And here, once again, we point out, you won’t even know this is an issue if you aren’t trying to UNIT test your code.