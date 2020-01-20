---
title: 4 Reasons to Write Loosely Coupled Code
tags:
  - DRY
  - loose coupling
  - single responsibility
  - tdd
url: 4050.html
id: 4050
categories:
  - Software Architecture
date: 2016-09-27 06:30:00
---

This past week I got into a small discussion about the importance of loosely coupled code.  Specifically, I was looking at event handler code that did nothing more than change the size of another element on the screen.  But the event handler code was in the controller which in the particular implementation we are using was the event handler portion of our Model in a MVVM architecture.  The question becomes does this code belong in the view, or does it belong in the controller? The question of where code belongs leads eventually to arguments for loosely coupled code.  If I put code in my controller that is manipulating the view, then I either need to mock out my view in order to test my controller or I have to have an instance of my view available to test my controller.  Having coded enough systems to know that both of those choices are problematic, I opt for placing the view specific code in the view.  Another place where you might place this code would be in a View Specific event handler class.  But that would only be for the purposes of clean separation.  Something we might consider doing if the View were plain HTML.  But in our particular case, the view is generated from code, so placing the handlers in that same code seems to be the most appropriate location for it.

But all of this leads to a larger question.  Why should your code be loosely coupled at all? <figure>![](/uploads/2016/09/image-2.png "4 Reasons to Write Loosely Coupled Code")<figcaption>Photo credit: [Hernan Piñera](//www.flickr.com/photos/hernanpc/7115374283/) via [VisualHunt.com](//visualhunt.com) / [CC BY-SA](//creativecommons.org/licenses/by-sa/2.0/)</figcaption></figure>

<!-- more -->

Refactoring
-----------

One of the main advantages of loosely coupled code is that this combined with granularity makes code refactoring easier.  Because my controller class only handles events that cause data to be manipulated and does not cause any changes to the view, I know that any changes I make to my controller will not cause any unexpected presentation layer changes.

Similarly, and in this case, more importantly, any changes I make to the view will not force me to make changes to the controller class.  That is, all of the code that could be impacted by any one change happen in one and only one class.

Loose coupling can also occur horizontally, and often does without us thinking too intensely about it.  Have you ever used a control from one vendor only to find another control works better?  The more easily those controls match each other’s programmable interface, the easier the job was to swap them out.  We can do the same type of thing in our own code.  The more granular the code is broken down; the easier any one part of the code can be replaced by something that works “better”.

More Maintainable
-----------------

But the argument might be make that this is a lot of work for something that isn’t very likely to happen.  But anyone who has been programming more than a few years on essentially the same system knows that it does.

Here’s how it looks.  You start out writing a system and you care very little about coupling or not.  Hey, we’ve got code to write and very little time.  Full steam ahead and forget best practices.  And for now, let’s assume you have a small enough system that you actually pull it off.  You get the system delivered, and somehow it miraculously works.  Congratulations.

A few months go by and you get a new requirement or a change request comes in for an existing requirement.  Either way, you manage to shoe horn the change in.

More time goes by and a change to the last set of changes comes in.  And now you realize that what you have is a rather fragile architecture that can’t possibly sustain the new request.  A change that might have taken a month given a more loosely coupled architecture is now going to take three because of the amount of rework that is going to be required.

Another way that loosely coupled code helps is when you are writing a very large system.  Let’s say the system is large.  You write this for a few months and everything seems to be going fine when you hit that first, “oops! we forgot” moment.  So you have to go back and retro fit the change into your tightly coupled code.  And then you progress some more and hit even more of those kind of changes.  Eventually, everyone learns to hate this system because it is so hard to work on and every change we make to existing code causes more and more bugs.

Cross Platform
--------------

Refactoring and maintenance have some practical implications beyond just being able to work on your code.  In this world of multiple platforms, you may find yourself wanting to run some of what you’ve written on another platform.  There are multiple ways you might do this, but keeping your code isolated is going to make this easier.  Say you want to run your code on the web, as a desktop application, on the various mobile phones and tablets that are available.  Even if you do this all in the confines of HTML, JavaScript and CSS, you will most likely have different presentation layers.  If you have view specific code (to use our example again) that isn’t in the view, you’ll need to write code in the controller that detects which of the platforms you are running on.

But if you’ve decided to use native components on each of the platforms, the view layer is actually going to change drastically.  And the view specific code you wrote might not even work on the new platform.  All the more reason to keep the view code isolated.

Cross Framework
---------------

Or, maybe you are going to upgrade your framework.  Angular 2 just released and it is drastically different from Angular 1.  I can tell you right now, the organizations who wrote loosely coupled Angular 1 code are going to have a much easier time transitioning to Angular 2.  Or maybe you want to switch from Angular to React, or from any of the multitude of frameworks to another.  The more loosely coupled your code, the easier it will be to move.  And nowhere is this more obvious than in the fast moving JavaScript world.

Single Responsibility
---------------------

Loosely coupled code is strongly related to the Single Responsibility principle.  You can’t have loosely coupled code unless the code follows the Single Responsibility principle.  And in the case above, putting view code in our controller actually violates both at the same time.  The controller is for manipulating view agnostic data.  The view is where your presentation layer code goes.  Of course, we could turn this around and remove the controller and put all of our code in the View.  Now that would definitely violate the single responsibility principle while giving the appearance of loose coupling.  But if we ever had to create multiple views we would quickly violate yet another principle.  Don’t Repeat Yourself.

Testing?
--------

Normally, I would lead off with testing.  But the argument could be made, “If I’m not going to write unit test, do I still need to do this?”  Actually, someone did make that argument.  And for now, ignoring that not writing unit test is a bad idea that I’ve written about before.  Writing unit test helps us think about these issues up front.  It isn’t that testing is the reason we write good code, although if that’s how you want to think about it I’m not going to stop you.  It is just that writing good test causes us to write good code.  Since it is the most visible benefit we tend to think of it first.  But testing isn’t at all why we do this.  We write test because it provides benefits much like loose coupling does.  Saying that we write loosely coupled code that follows the single responsibility principle so we can test would be like saying we don’t get lost so we can use our GPS.  It is the GPS that helps us to not get lost.  In the same way, it is the test that cause is to write good code.

Best Practices
--------------

I know I’ve been following best practices for so long that I often forget why I’m doing them.  They work.  I do them.  But every once in a while, I need to step back and take a look at what I’m doing and ask “Why?” once again.
