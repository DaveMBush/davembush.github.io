---
title: 3 JavaScript Fallacies You Might Believe
tags:
  - angular
  - javascript
  - performance
  - react.js
url: 4229.html
id: 4229
categories:
  - Javascript
date: 2017-03-14 07:30:00
---

You know, you think the whole world knows something is true until you hear someone people respect say something really dumb.  The three JavaScript fallacies I have here are actual statements I’ve heard over the last week during a discussion about Angular2 and Rect.  What makes these fallacies particularly interesting is that they sound plausible.  In fact, there are time when they are even true.  But in the larger context of a JavaScript application they are nearly always false.

So, here are 3 JavaScript Fallacies you may still believe that you may want to reevaluate.

<figure>![](/uploads/2017/03/image-2.png "3 JavaScript Fallacies You Might Believe")<figcaption>Photo credit: [bark](//www.flickr.com/photos/barkbud/4341791754/) via [VisualHunt](//visualhunt.com/re/8dc251) / [ CC BY](//creativecommons.org/licenses/by/2.0/)</figcaption></figure>

<!-- more -->

## Direct Access to the DOM is Faster than using a Virtual DOM

OK. I will grant that if you only ever want to change one thing on the screen at a time, yes accessing the DOM directly from JavaScript is probably going to be faster than going through some kind of Virtual DOM layer as is common in React, Angular2 and several other libraries and frameworks that are available today.

But, the fact is, that’s not how most code works. If you are writing this kind of application and you are using a library or framework that uses a virtual DOM layer, something is wrong. I would argue you’ve probably chosen the wrong library for what you are trying to do.

But, let’s assume that you are writing a typical SPA application that doesn’t just update one area of the screen. In this case, the fastest way to make this update is all at once. One call to the DOM from JavaScript. I’ve written before about how [slow accessing the DOM is](/javascript-performance-tweaks/). And for all the performance enhancements since I wrote that article, accessing the DOM is still one of the slowest things you can do. So, a framework that lets you write your code in a way similar to how you would write to the DOM directly, but lets you do this in a way that doesn’t actually write to the DOM until the last minute is FASTER than writing to the DOM directly.

## Immutable Objects are Necessarily Slower than Mutable Objects

Functional JavaScript programming has become the latest cool new buzzword in the JavaScript community, for a lot of good reasons, but one main concept that comes along for the ride is the idea of making all of our object immutable. This means, for example, that if I want to modify an array, instead of changing the current array, I would create a new array and copy the elements and the new element into it. No more push.

Similarly, for a regular object that isn’t a list, we would create a new object and copy the existing elements into it and then overwrite the items that have changed.

### Why?

Now, why would we want to go to all of this work? It does seem like it would be faster to just modify the existing object. Right?

But you see, there is this little thing called “change detection” that more than makes up for all of this overhead I just described.

In most of our applications, at some point we want to know if an object has changed, right? If we can’t rely on the fact that we have a new object, we have to do a deep comparison of the two objects until we’ve determined that there is a difference, or we’ve been through the whole list and verified that the nothing has changed.

Once we can rely on objects being immutable, we can use the equals operator (==, or ===) to see if the object has changed. I’m sure you can see that this takes much less time than evaluating an entire object tree, even if you object only has two properties in it that need to be compared.

### Real Problem?

But let’s step back and look at the bigger picture. In most of the code that I write, what we are really talking about is immutable arrays. Yes there are other non-array places where we would use immutable objects, but my guess is that immutable objects impacts 80% of my code. Get a list of records from the database for example.

The fact of the matter is, every time I access the database, I get a new array back anyhow. Even in the case of retrieving a single record, I still get a new object back.

If I need to delete an element from an array, here again, I’ll end up creating a new object.

In fact, except modifying a row, or adding a record to the end of a list, just about everything I tend to do with an array ends up being an immutable operation anyhow. By making everything immutable, we are forcing the areas that we aren’t already implementing immutability to be immutable. The point is, in terms of performance the net is an obvious gain both in terms of performance and in terms of consistency.

And, going back to the issue of rendering our data into the DOM, because the change detection is faster, we can determine that a particular component doesn’t need to have the DOM updated quicker rather than re-rendering the entire DOM.

## Backwards Compatibility means I can’t upgrade my Development Environment

Continuing on, remember the conversation I’m referencing was about React and Angular2. And the guy who was making these statements about performance was asserting that he couldn’t upgrade his React environment to use the latest and greatest tool chain because he had to support older browsers. He specifically stated IE6. Now, knowing the site he’s talking about, I doubt we make any money at all from people who run IE6. But, let’s just assume for a second that we do.

### So, what you’re telling me is...

... that you are willing to let performance suffer for 99% of your very large customer based because you need to support the 1% (I’m being generous here) of the people who have refused to upgrade and probably aren’t producing any revenue for the company? Maybe I’m missing something, but this seems rather short-sighted. That, or maybe you just don’t know that the new features in the newer browsers allow you to not just write better JavaScript but also allow you to do things without JavaScript that perform better. If you are really interested in writing fast web sites, you should be moving to the latest and greatest tool chain as often as is humanly possible.

### Bugs and Security Risk

Just this last week I saw a study that said that over a third of the web sites on the Internet were running code that left them venerable to security risk. That is, the code they were running had known security risk. By not upgrading your tools for public facing sites, you are leaving your company at risk. When your site is compromised, do you want to be the one who has to explain to your boss that it is because you refused to upgrade your tools? I sure don’t.

### But what about the old browsers?

Well, if you really need to support the old stuff, there are polyfills that you can apply that will let you run the newer stuff in the older browsers. Seems to me you get the best of both worlds. Your customers who are up to date benefit with a better performing, and less buggy website, and the customers using older browser still get to see something. Are there places where you might still have to make some compromises. Sure. There are a few places. But not enough that you shouldn’t upgrade.

## What is the Bigger Problem?

So, how can this happen? How can really smart people make really bad choices like this?

I don’t really know, but I have a few theories.

One is just plain arrogance. Being so sure you are right that you never stop to think you might be wrong.

I have to admit, when I first learned about immutability, I thought it sounded slow too. But, my thinking went along the lines of, “well, much smarter people than me are working on this, they must think it makes sense. I wonder why?” And then I started digging for answers.

So, I would encourage those who are responsible for making decisions to make sure that the people they are listening to can actually back up what they are saying and not just assume they are right because they seem so confident.
