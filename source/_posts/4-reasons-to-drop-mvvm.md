---
title: 4 Reasons To Drop MVVM
tags:
  - javascript
  - mvvm
url: 3980.html
id: 3980
categories:
  - Javascript
date: 2016-07-27 06:30:00
---

The MVVM design pattern has been around for quite a while now.  It has a lot of strengths when done correctly.

But, I believe the time has come to recognize that MVVM has a lot of shortcomings that point to its demise.  Since I primarily develop web applications, I will keep this discussion centered on the use of MVVM in web applications.  The use of MVVM for desktop may or may not have these same issues.

I realize that for some of you, the very suggestion of dropping MVVM will invoke a negative emotional response.  Some very smart people have quit their job at the suggestion that MVVM and its close cousin two-way data-binding, be abandoned in favor of another way.  But just for a few minutes, I would like for you to stop treating programming as a religion and consider the possibility that there may be a better way.

![image](/uploads/2016/07/image-2.png "image")

History of MVVM
---------------

MVVM was originally created by John Gossman to support the XAML syntax used to create Windows™ desktop applications and Silver Light applications.  Its main advantage has always been that it provides an easy way to decouple the View code from any business logic that might need to run.  Because of this decoupling, our applications become much easier to unit test.

The next major implementation of MVVM that I can remember is [Knockout](//knockoutjs.com/).  It is the knockout framework that introduced me to MVVM and I have to say it is also the only one I feel like actually got it right.  By that I mean that it actually did what it was advertised to do.  Maybe that’s because of all the implementations I’ve used, Knockout is the only one that ONLY implemented MVVM rather than making it part of a larger framework.

Definition of MVVM
------------------

I’ve written about MVVM before where I’ve explained more completely what MVVM is.  But just so we have a working definition of what I mean when I talk about MVVM, let’s define it this way.

MVVM is a design pattern that uses two-way data-binding to get data in and out of the presentation layer, referred to as the View, without the programmer needing to do any more than specifying that this should happen in the view.  MVVM is also able to have data elements and functions in the “ViewModel” track changes so that anything that is dependent on other data is automatically recalculated without the programmer having to write a lot of code to make this happen.  This creates a model that is able to respond to events in the view, but is primarily data centric rather than event centric has we have often reasoned about our applications in the past.

It sounds great.  And when it works it is.  But that’s the main problem, it hardly ever works well.

MVVM Done Right is Slow
-----------------------

If you’ve had any experience or paid any attention to the implementation of MVVM using JavaScript, you will realize that the number one problem with MVVM is that it is a memory hog and performs poorly for all but the most trivial of applications.  In fact, for all of the popularity of Angular JS, the biggest complaint has been around the implementation of the data-binding.  In a large application, you might need to loop through the data multiple times to make sure it has all recalculated correctly.  If you just use the framework and let the framework deal with your sloppy code, this can make the system incredibly slow.  If you actually pay attention to what you are doing, it takes longer to implement than if you had chosen some other design pattern.

But doing that means we have not moved on to…

MVVM is Hard to Implement
-------------------------

Recognizing that looping through the data until it stabilizes may not be a good idea, the framework designers have developed rules such as, “We’ll only run the digest cycle once.”  and “We’ll only run it when some user interaction has occurred.”  Well, OK.  That sounds good.  At least now it will be obvious that I have a problem.  But this is where the trouble begins.  If I can’t rely on my data, and ultimately my view, responding to changes in my data correctly, I am left with having to only partially implementing MVVM so that I can work around these limitations and using other means to make sure my view is updated correctly.

This is to say nothing of many frameworks just not working as you would expect them to.

MVVM is Hard to Reason About
----------------------------

Again, in all but the most trivial of applications, and because of the optimizations that various frameworks have tried to implement, MVVM becomes difficult to implement.  As I’ve tried to explain MVVM to others and even as I’ve tried to implement it myself, I’ve found that the simple act of keeping the view stuff in the view layer and the data stuff in the data layer and making sure it all updates appropriately has me, at times tearing my hair out.  Many times this is caused by incomplete implementations.  But if being hard to implement means it is hard to reason about, maybe we shouldn’t be using it to begin with.

MVVM is Overkill
----------------

But it does work sometimes.  In really simple CRUD applications, it works great.  None of the problems I’ve mentioned.  And this is the great seduction of MVVM.  You try it on some small application and you get excited.  Like a gateway drug, it lures you in.  And when you finally go to implement it on some larger application, you find out that it really doesn’t scale all that well.  And on that small app you tried it on, couldn’t you have done that just as easily using another design pattern?

Where to Go from Here
---------------------

As I was reviewing these arguments with a co-worker this week, he asked, “Are you saying we shouldn’t be using MVVM?”  And my answer might surprise you.

I said, “Given the two models we have to work within the framework we are currently using, MVVM is the best choice.” However, what we might want to consider is moving to another framework that provides a better design pattern.  There are several “One-way” design patterns that intrigue me.  The first is the basic Flux pattern that React tends to use.  Done correctly, this uses events to achieve the decoupling we all should be striving for.  At its core, it is basic MVC.

The second one, which is very flux like, is RxJS.  I’m still wrapping my head around how I to use it in an application and honestly don’t know enough about it at this point to say any more than that it looks interesting.

And even if we decided to move away from MVVM, I think using two way data-binding between the view and the ViewModel is good.  I just think the ViewModel shouldn’t try to re-compute the values as part of what it does.

Leave that to the developer to control.  The problem is, trying to get existing systems that implement two-way data-binding to only work at that level would not work correctly.  

### Other places talking about MVVM

*   [The Advantages and Disadvantages of MVVM](//blogs.msdn.microsoft.com/johngossman/2006/03/04/advantages-and-disadvantages-of-m-v-vm/) (by John Gossman himself!)
