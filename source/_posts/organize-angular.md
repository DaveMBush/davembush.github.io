---
title: How to Organize an Angular Application
tags:
  - angular
  - angular 2
  - design
url: 4358.html
id: 4358
categories:
  - Angular 2
date: 2017-06-20 06:30:56
---

Over the last couple of months, I’ve discussed specifics about Angular architecture.  Today, I want to discuss a more general question.  “Where do I put what files?” <figure>![](/uploads/2017/06/2017-06-10.jpg "How to Organize an Angular Application")<figcaption>Photo credit: [Marine Corps Archives & Special Collections](//www.flickr.com/photos/usmcarchives/9524920862/) via [Visual Hunt](//visualhunt.com/re/4307a6) / [ CC BY](//creativecommons.org/licenses/by/2.0/)</figcaption></figure>

<!-- more --> 

Directory Structure
-------------------

Let’s start with our general directory structure.

    app+
       + animations (holds common animation definitions)
       + components (holds common application components)
       + routes (top-level routes aka pages)
       + services (this is where you would put your services)
       + state (state management code.  One sub-directory for each reducer defined in app.stores.ts)

I use animations to transition between pages and for several locations where I expand and collapse panels.  I define the animations in the animations directory and import them into the components that need them.  Most of the sample code for animations you’ll find on the web defines the animation in the component’s TS file.  The problem with this is that it encourages redundancy. “Yeah, yeah, I’ll pull it out if I need it someplace else.” never works.  If there is even the remote possibility that you’ll need the code someplace else, break it out into its own file and import it the the file or files that need it. We’ll come back to components.  First, let’s look at routes.  You might prefer to call this directory “pages.” I call it routes.  The point is, this is where your top-level components for the routes go.  In my code, with all but the simplest of routes, my route components compose sub-components and orchestrate how data flows in and out of those sub-components.  These sub-components will either end up as sub-directories of routes or they will end up as sub-directories of components. The components directory holds dumb components that are common to two or more routes.  When we have components that are common to multiple projects, we try to break those out into separate project that we can npm install into our various projects. Next, comes the services directory.  This is where you might put any services you would create.  If you had pipes, those would go under a pipes directory, etc. Finally, the state directory.  Once again, you might prefer to call it ‘store’.  This is where our NgRX stuff goes.  One directory for each top-level entity and sub-directories under each of those for child entities.  I try to group the models, reducers, actions, and effects under the directories that represent the entities rather than having a directory for each type.  Trust me, I tried it the other way and that doesn’t work out all that well.

Components
----------

I’ve mentioned this in previous posts, but it bears repeating.  Your top-level route component should be the only thing that knows about your NgRX store implementation.  If you have sub-components that also listen directly to the store or fire actions to it, you’re doing it wrong.  Here’s why… You never know for sure that you won’t need that same sub-component in some other place in your system working off a different set of data.  You can be sure that your route component won’t get re-used.  And besides, we need something to be responsible for managing the data. Also, components should only display the data they are given.  NO Business Rules!  I fear this will be the hardest hurdle for most of you to overcome.  And, I realized earlier today that this is a valid concern because I’ve been here before.

Deja Vu
-------

A long time ago, in a galaxy not all that far away, there was a design pattern called, “Document/View.” This was a pattern that Microsoft had come up with for their C++ framework called MFC.  (Yeah, I know, I’m dating myself.)  The idea was, we would put Data stuff in the Document and the View would render the data.  This allowed us to have multiple Views looking at one Document class and the Views would render it appropriately.  It was cool little model except for the fact that people were confused about what went where.  It was clear what was presentation.  And it was reasonably clear what was data.  But there was this part of the system that no one talked about called business rules.  Where did they go? Fortunately, Document/View died.  And with NgRX and Redux, we DO have a location for our business rules.  Reducers, @Effects, or something an @Effect would call.  Keep that fixed in your brain and no one will get hurt.

State
-----

While I was training someone this week, the question came up about moving the State stuff under the routes because we had established a one-to-one relationship between the routes and the entities in our store.  But, that’s just how we happened to setup our project because it was a demo.  Just like with Document/View, it is entirely possible to have multiple Routes looking at the same Entity.

Clean Separation
----------------

The point is, we want clean separation between our presentation code, our business rules, and our data.  Having Reducers and @Effects keeps the business rules separate from our state cleanly enough.  But mixing all of that in with our presentation code is just asking for trouble.

Functions vs Classes
--------------------

The saying goes, “When all you have is a hammer, everything looks like a nail.”  And no where do I see this more than with programmers coming out of an Object-Oriented programming model trying to work with JavaScript or any of the variants, especially TypeScript. The great thing about TypeScript is that it makes JavaScript FEEL a lot more like Java or C#.  The bad thing about JavaScript is that it makes it easy to forget that it isn’t Java or C#.  Here’s what I mean. I’ve seen guys create a TypeScript class so they can new up what would otherwise be an Object Literal. I’ve seen guys create a “Utils” class so they have a place to put helper functions when they could otherwise just create a TypeScript file that exported JUST that function.  BTW, this completely circumvents the whole, “what do we call this class other than Util?” problem!

We’re All New Here
------------------

Hey!  Angular is still relatively new.  All I’m doing here is sharing my thoughts on what works and what doesn’t, based not only on my experience so far with Angular, but on my 30 years of experience programming.  What are some of the issues and best practices you’ve discovered so far?  Leave me a comment.
