---
title: NgRX/Store Coding Sanity Epiphany
tags:
  - angular
  - design patterns
  - NgRX
url: 4346.html
id: 4346
categories:
  - Angular
date: 2017-06-06 06:30:44
---

Maybe this is all obvious to you, but since I don’t see anyone talking about this when I search for “how to do NgRX” or the various variants, I thought I’d call it out in this weeks post. If you haven’t been following along, you’ll want to review [my previous posts on NgRX](/tags/ngrx/). <figure>![](/uploads/2017/06/2017-06-06.jpg "NgRX/Store Coding Sanity Epiphany")<figcaption>Photo credit: [spcbrass](//www.flickr.com/photos/spcbrass/394867154/) via [Visual Hunt](//visualhunt.com/re/cff786) / [ CC BY-SA](//creativecommons.org/licenses/by-sa/2.0/)</figcaption></figure>

<!-- more -->  If it isn’t clear yet, I’m still sorting out a lot of this Angular/Redux/NgRX stuff.  But as I was working on my current project this week, I realized I have WAY too much code in my presentation layer.

The Epiphany
------------

I have two main screens I’m working on.  As you read the articles on the Internet that explain how to use this pattern or the Redux pattern it was derived from, you’ll quickly learn that you want to work with a “Smart Component.”  This component is kind of a traffic cop.  It uses observables to listen to state change in your Store and it sends actions to, primarily, update the database and change the store’s state.  From what I’ve been able to gather, the expectation is that a lot of the logic that would be needed to actually process the data is going to go in this smart component.  The first screen I worked on, followed this basic pattern.  This put a whole crap load of code in my smart component. On the second page, I kind of stumbled onto what I believe is a cleaner model.  I realized that I was hanging onto data in my smart component that was also in my store.  That seems kind of dumb.  If all I need is in my store, why not just fire off an action to do whatever it is I want to do and have the @Effect grab the data from the store? This is why I ended up making my smart component listen to the observables and dispatch events to the store based on changes in my components.  This includes things like button clicks.  Any other processing that needs to take place takes place in either an @Effect or is called from an @Effect. I can’t describe for you how much cleaner my codebase is as a result!  WOW! But, will it work on the first page the same as it works on the second page?

The Test
--------

You see, this is a big difference between the first page and the second page.  The first page is basically a search and list page.  The second page is an edit page for an item.  On the first page, I had multiple store entities for the various parts.  I had an entity for the search fields.  An entity for the search results. And others.  Let’s just say my model isn’t very flat. The fact of the matter is, the second page that I created wasn’t really all that flat either.  But because I started with the concept of not putting any logic in my smart component, it felt easier to manage. So, the first thing I wanted to do was to create a reducer for the page.  All this reducer will do is distribute the action down into sub-reducers.  This allowed me to keep all of my action code the same.  The only thing that changes is that the directories for my sub-reducers and the @Effects, Actions and Models (interfaces) that are associated with them go under my directory for my main Reducer and Model. I still have a bit of code that I’d like to clean up, but on the whole, I like this pattern much better than what I was doing before.

Advantages
----------

The main advantage to using this new architecture is that it simplifies and reduces testing considerations. For example, because all my presentation layer is now doing is either reflecting the state that is in my store or telling my store to do something, there really isn’t much, if anything, left to test in my presentation layer.  If you’ve written your code correctly, none of the methods in your view should have a cyclomatic complexity of greater than two.  You may still want to write some end-to-end tests to make sure that the NgRX/Store loop is working correctly.  But that is an entirely different subject. This does not mean that we don’t have to test anything.  All of that code had to go some place, right? But, here’s the deal.  Because the code is in an @Effect or a Service (generally) your tests become much more simple.  You might have to dummy up a store or a service.  But for the most part, your tests won’t really look much different that tests you would write for regular JavaScript code without a framework. The other HUGE advantage to using this architecture is that it allows you to distribute your code so that no file is too large and hard to reason about.  It allow you to follow the “Single Responsibility Principle” in greater granularity than you might otherwise be able to do. And finally, this architecture allows you to treat all of the component code: the html template, the CSS, and the TypeScript file, as all View code.  And I think this is where many people are confused about Angular.

View Confusion
--------------

In a MVC or even an MVVM pattern, we’ve also considered the HTML template the “View” and the JavaScript (or in our case, TypeScript) code the controller.  This is a common misconception that I believe the ASP.NET crowd still gets wrong.  Code-behind code isn’t your controller.  It is helper code for your View.  And so, we end up putting processing code in our view, when it really belongs in an entirely different file.  This is what the Model View Presenter pattern solves.  If you aren’t going to use NgRX and Reactive Forms, you should check out MVP as a way of architecting your code using the older Template Driven Forms approach that was common in AngularJS.

Code
----

For the purposes of this article, I’m going to assume you’ve read my other articles which I’ve linked to at the beginning of this post. So first, the basic directory and file structure of this new method might look something like this: https://gist.github.com/DaveMBush/b79c333224340b6a9d96ed719b116112#file-directory-structure-txt Some things to note:

1.  Your Actions are defined in the target.  You would seldom, if ever, define an action at the route level.
2.  Effects are optional, just like any other time you would use them.
3.  Effects are seldom, if ever, defined at the route level.
4.  I’m using “route1” etc and “sub-reducer1” etc as sample names.  Use names that represent your route names and the data you are storing.
5.  The only reducers that gets defined in our app.store.ts file are the reducers in the route directories.
6.  You still need to register each of your effects in app.store.ts as you have been doing.

The next thing that is probably not clear is that your top level model, ie “route1.model.ts” should only hold the sub-reducers.  I’ve also found it useful to make all of my top level properties optional. https://gist.github.com/DaveMBush/b79c333224340b6a9d96ed719b116112#file-route1-model-ts And this is used in your route reducer as: https://gist.github.com/DaveMBush/b79c333224340b6a9d96ed719b116112#file-route1-reducer-ts Now, the trick we need to implement is that we need to delegate the actions down to the appropriate reducers and we only want to change the state object to a new object if a child state has changed. In the top level reducer, you need to put code that looks something like this: https://gist.github.com/DaveMBush/b79c333224340b6a9d96ed719b116112#file-route1-reducerb-ts The key here is that you want the property names in the reducerList to be the same name as what is in the Route1Model and you want the values assigned to them to be the function pointer (notice, no parenthesis) that should be called. The actual sub-reducers look like a regular reducer.  The only real difference is that you will be calling the function that returns the state, the second export statement we normally put in our reducers that returns the ActionReducer<> is not needed. So, our Object.keys().map() processes each reducer and updates the parent object if the child has changed. Now, by way of reminder.  You can observe all of the store, or part of the store.  So, your smart component might observe just a sub entity or the whole entity depending on the need at the moment. Finally, lets say you want to have a “Save” button that causes the information in your store to be persisted to a database.  You would place a method in your smart component that gets triggered by the button and fires a “Save” action to an @Effect. https://gist.github.com/DaveMBush/b79c333224340b6a9d96ed719b116112#file-save-ts Your @effect will respond, and since @Effects typically already have a store injected into them, you can use the store to retrieve the data. https://gist.github.com/DaveMBush/b79c333224340b6a9d96ed719b116112#file-effect-ts Notice that since we are doing our own dispatching inside of a subscribe() we return {type: ‘noop’} from the @Effect.  This is because ALL @Effects MUST return an Action unless they have been turned off in the constructor.

Conclusion
----------

Well, that’s the basic idea.  What do you think?  Leave me a comment.
