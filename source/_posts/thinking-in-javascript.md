---
title: Thinking in JavaScript
tags:
  - javascript
  - MVC
  - mvvm
  - NgRX
  - redux
url: 4431.html
id: 4431
categories:
  - Javascript
date: 2017-09-05 06:30:54
---

Over the last week I've gradually come to the realization that the fundamental reason why most people have trouble with JavaScript is because it doesn't fit their mental model of how programming should be done.  This isn't to say that most programmers don't manage to achieve their end goal.  But if you sit back and take an objective look at the code we end up writing, you have to admit, the code ends up being quite ugly. Now, this isn't a dig at the way we've been doing things.  We've all been doing the best we can with what we have.  But, the JavaScript world has progressed and there is a better mental model that has developed and should even be expanded which will allow us to develop more complex and feature rich applications now and well into the future. <figure>![](/uploads/2017/09/2017-09-05.jpg "Thinking in JavaScript")<figcaption>Photo credit: [freddie boy](//www.flickr.com/photos/froderik/8283727226/) via [Visual Hunt](//visualhunt.com/re/fb4c57) / [ CC BY-SA](//creativecommons.org/licenses/by-sa/2.0/)</figcaption></figure>

<!-- more --> 

Where We've Been
----------------

I've been saying for years that the thing that holds most programmers back is that they always want to treat whatever new thing they are using like the last thing they were using.  And nowhere has this displayed itself more apparently than with JavaScript. Take the most obvious of examples.  Everyone knows, or should know by now, that JavaScript is not really object-oriented.  And yet, we've been trying to force JavaScript to BE object-oriented pretty much from the beginning.  This hasn't been such a big problem, although one could argue that by trying to make JavaScript object-oriented, we've prevented it from being able to do some of the things it does best. Where we really run into trouble is with the event based, and often asynchronous nature of JavaScript.  Think about this.  For years, we've been trying to synchronize something that is inherently asynchronous.  And this is where the real trouble begins. First, we had call back.  Then promises.  Now Observables.  Soon async and await.  And while callbacks are how the asynchronous nature of JavaScript is handle under the hood, the others are attempts to tame the asynchronous beast.  Especially async and await.

Why?
----

Now, this is where we are.  Constantly trying to make JavaScript be something it isn't.  But, why is this? I believe it is because we are trying to impose models onto JavaScript that were useful in our desktop and server-side applications.  MVC, MVVM, Object-Oriented, and others all grew up in a world that was both synchronous, multi-threaded, and lent themselves well to an object-oriented model.  As various frameworks have evolved, the attempt has been to take these familiar models and apply them to an asynchronous, single threaded and not really object-oriented.  From where I sit, I am amazed any of this worked at all.  It seems to me it should have failed long ago.

A Light in The Darkness
-----------------------

Hey, I've been stuck in the old school model too.  But, I'm starting to think there may be a better way.  I've written about Redux and NgRX a lot on this blog.  I've fielded a lot of questions on the Angular slack channel.  Most of the questions revolve around the basic question of handling multiple asynchronous calls for data as part of one action.  All of the questions presuppose you would need to make each of the calls for data and then use some method of waiting for everything to return and assemble the data before moving on.  In each case, I recommend an alternative.  What if, each call was a unique action.  When each returns, another action is fired that places the return data in the appropriate store, or sub store.  In this model, we don't care when the data comes back.  When it comes back, we deal with it appropriately.

An Example
----------

Let's go with one of the more common examples I see. I need to make a request for a set of records.  Once I have the results, for each record in the result, I need to go get a set of child records.  Here is how I would deal with this at a very high level using NgRX.  I'm sure this would work for multiple Redux patterns but they may call things by different names.

*   Fire an Action that request the main set of records.
*   The appropriate Effect responds to the action by making an AJAX call for the data.
*   When the AJAX call returns,
    *   fire an Action that puts the main records in the store.
    *   for each record in the result fire an Action asking for the child record(s).
*   The appropriate Effect(s) responds to the request for child records by making AJAX calls.
*   When the data returns fire an Action that places the data in the store.

Since your view is listening for changes on the entities in your store, it will update as the data comes in.  Even better if you setup a debounce on your listener, the screen will update only when all of the data has been retrieved.

The Key Concept
---------------

The key concept here is that we no longer care WHEN something happens.  We only care THAT it happens.  And rather than trying to setup forkJoins() or some other mechanism to flatten this all out, our code ends up being quite simple.  Discrete bits of functionality.  And now, all our asynchronous code becomes Reactive code.  We no longer need to flatten anything out.

Server Side
-----------

Sadly, on the server side, things aren't quite so easy.  At best we are tied to an implementation Observables and the various methods of combining Observables.  But I could also see some kind of client/server implementation that used a framework like SignalR or Socket.io so that as the various Observables complete, the data on the client would get updated.  An interesting way to make all of the AJAXy calls rather transparent to the user.

Taking it To the Next Level
---------------------------

You may call me a dreamer, but what if we made a JavaScript framework that was all message driven and reactive like I've described above?  We've done it before. When Windows was first created in ran on single core CPUs.  It was essentially single threaded.  The way it worked was primarily by putting "events" on a que and then sending the events to the appropriate application that needed to know about them. If you applied this model to JavaScript and folded in what I've described above, you could easily have a system that appeared to be multi-threaded even though it was single threaded at its core.

Don't Throw the Baby Out ...
----------------------------

Now, you may think I'm endorsing throwing out object-oriented JavaScript.  Actually, I think most of the View stuff we do lends itself well to object-oriented programming.  But, most of our business rules lend themselves better to the model I've described above.  Functional and Reactive.

What Do You Think?
------------------

Once again, I ask, what do you think?  I'll admit that this is just a germ of an idea.  Something you would add?  Some hole in my thinking?  Let me know in the comments below.
