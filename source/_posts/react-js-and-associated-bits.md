---
title: Reactions to React JS and Associated Bits
tags:
  - flux
  - javascript
  - react.js
url: 3850.html
id: 3850
categories:
  - Javascript
date: 2016-03-17 08:30:00
---

I’ve been learning React JS over the last several weeks.  Currently, I now know 4 of the major JavaScript frameworks: Angular 1, Angular 2, EXTjs (4.2 – 6.0.1), and now React JS.  To be clear, I also know Knockout and JQuery.  But I don’t consider these frameworks so much as libraries.  They’ve helped me understand the principles used in the frameworks, but they are not frameworks.  What follows is a summary of what I consider React’s strengths and weaknesses.

![React JS](/uploads/2016/03/image-3.png "React JS") Photo credit: [kristin osier](//www.flickr.com/photos/steffen-fam-pics/5472880836/) via [VisualHunt](//visualhunt.com) / [CC BY](//creativecommons.org/licenses/by/2.0/)

<!-- more -->

What is React JS?
-----------------

To be fair, React JS is one of those frameworks that borders on being considered a library.  This is because React itself is only concerned with the presentation layer of your JavaScript code.  However, as I learned over the last several weeks, if you are going to use React JS properly, you’ll also end up using some kind of Flux pattern.  You’ll also need some way of making AJAX calls.  And while each of these are separate decisions you’ll need to make about which library you want to use for these parts of the stack, they are all part of the React coding philosophy.  So React, the philosophy, is more like a framework. React JS, the library, is more concerned with presentation.  And what most people will tell you about React JS is that any DOM changes it makes are all made to a Virtual DOM instead of writing directly to the DOM.  By doing this, screen updates can be bundled into one change and are only made when they are needed.  This is in contrast to most other libraries that allow you to write to the screen directly. As I’ve written before, [the fewer times you can update your DOM from your JavaScript, the better performance you will see](/javascript-performance-tweaks/).

React JS Pros
-------------

So the first main benefit to using React JS is that you gain better performance because you aren’t writing directly to the DOM when you make a change to the presentation layer.

But, that isn’t what I would consider the best benefit of using React JS.

### Turtles All The Way Down [*](//en.wikipedia.org/wiki/Turtles_all_the_way_down)

You see, the reason that React JS can avoid writing to the screen directly is because it puts all of the presentation code in JavaScript using a syntax called ‘JSX’.  Now, as several places I was learning React JS from pointed out, this isn’t as ‘wrong’ as it sounds.

In Angular, for example, we are accustom to putting JavaScript in our HTML.  Here, we are putting HTML in our JavaScript.  In both cases, the View layer is separate from our business logic or data access (or should be) so we have not violated the Single Responsibility Principle in any way using either approach.

However, have you tried to unit test code you’ve placed in your HTML?  It is not easy, although I think I have a way that might make that easier now that I’ve done some work with React JS.  But, because everything is JavaScript, it is very easy to mock out a child component and actually Unit Test the presentation layer one component at a time.  In fact, if you concentrate on testing as you go, you will be forced to create very small components that you then compose into your pages.

In fact, it is the testing story that makes React JS my preferred framework right now.

### Testing

And while we are on the subject of testing, you might wonder how you test presentation layer stuff.

The React JS guys have created a test framework based on Jasmine called Jest.  The extensions in Jest let you render a component into a “fake DOM” using JSDom.  From there you can test to make sure the HTML you were expecting got rendered correctly and fire events and test to make sure that what you expected would happen actually happened.

What it doesn’t do is let you know that the component rendered in the way that you were expecting.  There are other, higher level tools, that are already available to do that.

### Super Loose Coupling

The React JS community refers to this feature as “One Way Databinding” and of all the concepts I had to figure out while I was learning how to program using React JS, this was probably the hardest to get my head around.

When you first hear, “One Way Databinding” you immediately start thinking, “How does that even work?  Eventually data has to get from the view down to the database and from the database back up to the view.  That’s two ways.”  But what they actually mean by “Two Way Databinding” would better be described as “Event Based Data Flow” or at least “Circular Data Flow” In very simple terms, the View fires an event to a “Dispatcher” which is a singleton.  Each repository, or data store, or model (just depends on what you want to call it) registers a listener with the “Dispatcher” that lets the dispatcher know that it wants to know whenever something significant happens.  These repositories are also singletons.  When the Dispatcher receives a notification from a View, it notifies all of the listeners in turn.  The listeners look at the message they receive from the dispatcher to see if it is something they care about.  If it is, they process the message accordingly.  Once they are done, they fire an event to each ControllerView that has registered a listener with them.  The ControllerView then updates the view based on the information it was passed in the event.

I don’t want this to get too far down the road of “How” but to make the above paragraph just a bit clearer.  There is a top level View item that does no rendering.  It is only responsible for responding to event notifications and passing the data down into the child views.  You may hear this referred to as a ViewController, but it is more accurately a ControllerView.

Because everything is basically an event (yeah, I know, not really an event in the strictest sense of the word) we can test each layer independent of the other.

### More Control

The final major advantage that I can see with using the React coding philosophy is that you have a lot more control over when things happen.  No longer are you at the mercy of how and when the framework you are using decides to update values.  If you need to update a value or update the screen, you can do that when you want to, as you want to.

And, because the only framework you are locked into when you are using React is the React JS framework, if you want to use some other implementation of Flux or AJAX, you can use whatever works for your situation.

React JS Cons
-------------

With all of what I like about React, there are some things that almost made me give up.

### Documentation

The week prior to learning React, I learned Angular 2.  I got spoiled.  I have to say, the Angular world seems to have MUCH better documentation.  Maybe this is because they’ve kept things relatively the same between major releases.  So you know, “this documentation belongs to this version.”  As I was learning React, I was never sure if what I was reading or what I was learning was currently the way things worked today.  Even on the main site, the documentation doesn’t seem to be up to date.  I’m pretty sure I could have learned a lot faster if I hadn’t tried to write Unit test at the same time.  Jest is where the documentation seems to be the weakest.  But I had challenged myself to approach learning React in a way different from what I normally do.

You see, normally, I use the excuse that “I don’t know the framework yet.” as a reason why I shouldn’t write Unit Tests as I go.  But this time, I decided that writing Unit Tests would be part of the learning.  So, before I could write my first view, I needed to be able to write my first test.  And that is when I realized this was going to take a little longer than I was used to.

### Not Highly Opinionated

OK, this one could go both ways.  Not being opinionated might be considered a good thing, and I addressed that above.  But, not having one right way to do something is going to be an issue for most large organizations.  In some organizations even Angular, which I would consider pretty opinionated, isn’t opinionated enough.

But, because React isn’t opinionated, and there is no clear direction in the documentation on how to DO Flux, you can end up getting multiple opinions on how to code Flux as you learn from multiple people.  Which can be confusing.

If you decide to go with React, just realize that you’ll need someone on your team who REALLY understands React and can make these decisions for your organization.  In my view, at the corporate level (vs the individual level) someone has to impose architecture OR you need to use a tool that has already imposed it.

### Do You Know React?

Similar to the opinionated issue but different enough that we should break it out.

If I put React on my resume.  What does that mean?  What will the hiring manager expect?  He’s been told that the application uses React.  So, he’s looking for a React guy.  But that isn’t all this guy is going to know.  And frankly, React is the easy part to learn.  Is he looking for a guy who knows the React philosophy?  What exactly is the React philosophy? For example, as I was learning, one of the guys was proposing that the store would notify the view that the data had changed, at which point the view would pull from the store.  But, why not just send the data to the view as part of the notification?  Wouldn’t that more loosely couple the architecture?  Both ways are using a React philosophy.  But, I would hope, that only one way is the way that is implemented at any one organization.

Conclusion
----------

Unfortunately, it will be a while before I can actually use React on a real project.  But knowing React, especially the testing end of it, has already influenced the project I am currently working on.  I need a few more weeks yet to make sure there aren’t any snags in how I am doing things, but you can bet the influence will show up in future post.
