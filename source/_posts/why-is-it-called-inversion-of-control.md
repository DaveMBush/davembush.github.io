---
title: Why Is It Called “Inversion of Control”?
tags:
  - design patterns
  - inversion of control
  - ioc
url: 3120.html
id: 3120
categories:
  - Software Architecture
date: 2015-06-11 06:00:00
---

![CHIL0007](/uploads/2015/05/CHIL0007.png "CHIL0007")

There is a guy I’m working with who is trying to wrap his head around design principles.  He’s been watching a lot of [PluralSight videos](/pluralsight).  As he was processing information about [Inversion of Control](/pluralsightIoC), he asked the natural question I’ve never actually considered before.  “Why is it called Inversion of Control?  Normally, when you talk about ‘Inversion’ you are talking about reversing something or negating something.  That isn’t what we are doing here.”

<!-- more -->

What Is Inversion of Control?
-----------------------------

So, let’s start with a definition of what Inversion of Control is.  And let’s start the definition by explaining what it isn’t. In the old days, back when I started programming.  We had this thing called “Procedural Programming.” Control of your application flowed from beginning to end with looping to take care of any kind of waiting that needed to take place.  [Martin Fowler describes this](//martinfowler.com/bliki/InversionOfControl.html) using an illustration that is version similar to a basic “How to program in C#” console application that is in the beginning of most beginner programming books.

> Let's consider a simple example. Imagine I'm writing a program to get some information from a user and I'm using a command line enquiry. I might do it something like this
>
>   #ruby
>   puts 'What is your name?'
>   name = gets
>   process_name(name)
>   puts 'What is your quest?'
>   quest = gets
>   process_quest(quest)
>
> In this interaction, my code is in control: it decides when to ask questions, when to read responses, and when to process those results.

What Inversion of Control does is that it puts something other than the main code in charge of doing critical parts of the program so that those other parts of the program can be swapped out by the person implementing the code.

Examples of Inversion of Control
--------------------------------

If you’ve ever written a WebForms or Windows Forms application, you’ve already experience inversion of control.

### OnPageLoad() or OnFormLoad()

You have probably never even given this much thought.  You think these two methods are where your code starts.  But the reality is, the code started long before these methods ever got called.  The thing that is in control is the .NET framework.  You’ve created a delegate that gets called by the framework so that your page or form does what you want it to.  Without this inversion of control, you would have to write a whole lot more code.

### Event Handlers

While you might think of OnPageLoad and OnFormLoad as event handlers, the way they are implemented are more like virtual functions.  Event handlers, on the other hand, are a specific type of delegate that say, “When this thing happens, let me know about it.”  The code that fires the event doesn’t even really care what you do during the event.  Again, the control has been “Inverted” because there is a lot of code that is running that you have no control over and probably have no knowledge of.

### Virtual Functions

One of my favorite methods of inverting control is to use virtual functions.  I can have a parent class that controls the flow of the program that calls various virtual functions in order.  But my child class can provide the implementation of the functions to make them do various things.  I used this, at one time in the past, to create a framework for submitting links to various bookmarking sites.  They all worked essentially the same way, all I had to do was implement the specific details for each site while 80% of my code stayed the same in the base class.

### Delegates or Callback Functions

Another common way of implementing inversion is by using delegates or callbacks.  This work in a similar way to virtual functions expect that the functions are typically passed in to a method.  Much of the current implementation of JavaScript uses this method of inverting control as a primary pattern (but not limited to it as the only way.)

### Dependency Injection

I’ve [talked about Dependency Injection before](/dependency-injection-not-di/).  Again, it is another way of delegating control to something other than the code that has main control.  What I want to make special note of here, though, is that Inversion of Control is not Dependency Injection but Dependency Injection is a form of Inversion of Control.  I think this is where most of the confusion about Inversion of Control comes from.  Most of the literature that talks about Inversion of Control talks about it at the same time they are talking about Dependency Injection.  As though the two concepts were the same.

Why Inversion of Control Matters
--------------------------------

Finally, I want to talk for a bit about why Inversion of Control matters.  Because, for me, it is a lot more important that we understand what we are trying to achieve than it is that we’ve given it the right name or that we even know what name to call it.  All of the examples I’ve provided are things you’ve probably done in your code.  Some of them you’ve done without even knowing you were doing them.  But think about the alternative of not doing them. First, your code would be a lot harder to maintain.  Just think of how hard it would be to write a web site and maintain it if you had to write code that handled all of the work that happens for you behind the scenes. Second, it makes the code more loosely coupled.  And by doing that, it makes the code testable AND it makes the code more flexible and easy to change in the future. Maybe instead of calling it Inversion of Control it should have been called Delegation of Control.  Either way, the concept is to remove as much code as possible from our main controlling code and give the implementation responsibility to something else that can be swapped in without having to change the main loop.
