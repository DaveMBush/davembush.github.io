---
title: Awesome Angular2 Architecture Options and Opinions
tags:
  - angular
  - archietcture
  - javascript
url: 4157.html
id: 4157
categories:
  - Angular
date: 2016-12-27 07:30:00
---

On the subject of Angular2 Architecture, the perception is that Angular 2 is a highly-opinionated architecture. But even though there is a [style guide for Angular 2](/angular.io/styleguide), there are a lot of decisions that still need to be made when working on any but the most trivial of applications. And even then, since most applications take on a life of their own, one could make the case that you need to make these decisions for any application you are building regardless of the initial size. Applications grow up. But, that’s another blog post

I’ve identified, and have formed opinions about 5 areas that Angular 2 leaves open for decisions. Areas that if you don’t spend time considering the choices and making decisions could cost you in the future

The five areas I’ve identified are:

1. Handling Forms
2. Page State Management
3. Component State Management
4. Data Flow
5. Client Side Data

<figure>![](/uploads/2016/12/image-2.png "Awesome Angular2 Architecture Options and Opinions") Photo via [africaniscool](//pixabay.com/en/users/africaniscool-216435/) via [Visualhunt.com](//visualhunt.com/photos/business/)</figure>

<!-- more -->

Handling Forms
--------------

There are, at this point, two ways of dealing with forms in Angular 2. The first looks a lot like Angular 1 in that it is “Template Driven.” That is, everything you would describe about the form goes in the template. Using declarative syntax, the bulk of your form logic is declared in “HTML” like syntax and bound to your TypeScript code in a “code behind” kind of way. In a lot of ways, this will be very familiar to people who have coded ASP.NET or Angular 1

But, the problem with using this method is that at some point, you won’t be able to do something you need to do using just a declarative syntax. The option is to use a “Model Driven” approach. These leaves the HTML parts in the template with a few tags to wire the template to the TypeScript code it is associated with, but the bulk of the processing is all in the TypeScript file. On the surface, you might think, “but Template Driven is much easier to think about.” But I assure you, using a predominately model driven approach has several advantages that the serious programmer will enjoy.

### More control over your forms.

The first advantage you will notice is that you’ll have more control over your form. One place you will notice this is with form validation. But, you’ll also notice greater control because you will have direct control on how the data moves in and out of your form instead of the “magic happens here” of data binding that can, with complex forms, become entirely too complex to reason about.

### Easier to test the forms.

Another huge advantage to using Model Driven forms is that you end up with forms that are MUCH easier to test. You can assume that the HTML is doing what it should and just test the TypeScript code. With a more template driven approach you’ll need to work out how to actually test your HTML. It can be done, and it isn’t really all that hard. But using a model driven approach is easier.

### Easier to reason about how the code is being processed.

Related to the two previous points, using Model Driven forms makes your cod much easier to reason about. Once again, that whole “magic happens here” approach of data-binding can get in the way, while using the model driven approach will allow you to be very direct about what gets updated when more directly.

Page and Component State Management
-----------------------------------

The temptation is to try to architect an Angular 2 project so that it looks something like how we used to write Angular 1 applications. That is, using an MV* architecture. Where this gets messed up is that the HTML template, the TypeScript and the CSS are really all part of the same class. Once you start thinking of them as one, MV* stops making as much sense

The pattern I prefer here is one that uses the top-level View as a Controller View. That is, it is the one component that is responsible for being the traffic data cop. All the components under it are responsible for either rendering the state information they have been passed by the Controller View, updating that state information or firing event out when they’ve done something the outside world should know about

The View Controller, on the other hand, is responsible for getting the data to and from where ever it needs to go

By doing this, you end up with very testable, modular code and it becomes very clear that all your logic for a page, or sub-page, exist in one very well-defined section of your code. In fact, you can eliminate the need for most dependency injection by following this pattern. Any dependency injection you do need will probably end up in your Controller View.

Data Flow
---------

So far, the three main methods of data management that have emerged for Angular 2 seem to be:

1. Direct Access
2. Flux/Redux
3. NgRX/Store

### Background

#### MVVM

While MVVM was possible in Angular 1, and it works at the View level in Angular 2, the preferred model is what has come to be known as “One way data binding” which sounds odd, and really doesn’t describe what it does

In short, while all the code you write may act like it is using two-way data-binding, the reality is that the code is only ever flowing in one direction

The problem with true MVVM data binding is that when it is done correctly, data changes because other data changed

This makes it very difficult to reason about the data in your application in all but the smallest of applications

Further, to get this to work correctly, the resulting system is almost always slower than it needs to be. I’ve written before about [why I think MVVM is a poor choice for design patterns](/4-reasons-to-drop-mvvm/).

#### Direct

It is possible to write an application that kind of looks like old style three layer architectures that some might try to call MVC, but it is a poor man’s implementation at best, and only because Angular 2 implements its own Dependency Injection Container mechanism does the result end up being anything close to loosely coupled

This implementation generally has the top-level component managing the state of the application, or at least the state of that particular page, and calling out directly to services that retrieve data from the server and manipulate data

While it works, in larger applications it can be difficult to manage and respond to state changes throughout your application. Imagine if that could happen for “free”.

#### Flux

The React community introduced a new pattern called Flux. There are multiple implementations of Flux, but the one that has become the defacto standard is called Redux. In general, Flux is made up of a series of publish subscribe mechanisms and ends up looking a lot like what the Gang of Four originally defined MVC to be while not actually being MVC.

In very simple terms, the View fires an event to a “Dispatcher” which is a singleton. Each repository, or data store, or model (just depends on what you want to call it) registers a listener with the “Dispatcher” that lets the dispatcher know that it wants to know whenever something significant happens. These repositories are also singletons.

When the Dispatcher receives a notification from a View, it notifies all the listeners in turn. The listeners look at the message they receive from the dispatcher to see if it is something they care about. If it is, they process the message accordingly.

Once they are done, they fire an event to each Controller View that has registered a listener with them. The Controller View then updates the view based on the information it was passed in the event. I don’t want this to get too far down the road of “How” but to make the above paragraph just a bit clearer...

There is a top-level View item that does no rendering. It is only responsible for responding to event notifications and passing the data down into the child views. You may hear this referred to as a View Controller, but it is more accurately a Controller View.

Hopefully, you can see how this solves the problem up needing to force an update on a View because some other View changed the state of something. Because everyone who cares about the state is listening for a notification that something changes, the screen update “just works” and is much more reliable than a more MVVM style of updating the view and data.

#### NgRX/Store

Reactive Extensions are available for multiple platforms, including JavaScript. You can read more about them at [http://reactivex.io/](//reactivex.io/) and I’ve written about [the reasons you want to use them](/reasons-to-use-rxjs-today/) before. But for purposes of this article, one of the problems that Reactive programming solves is cleanly dealing with the asynchronous nature of JavaScript

Nowhere is this more obvious than with Ajax request.

If you’ve ever needed to deal with having to wait for multiple Ajax request to complete before you do something meaningful with the data, you are going to love Reactive programming.

Another thing that Reactive programming gives you is it makes everything a “stream”.

In simple terms imagine working with an array that never ends and being able to respond appropriately to each element that come through that stream of arrays and you’ll have a good conceptual idea of what it means that everything is a stream. This is how Reactive programming deals with asynchronous calls and events. Add to this the concept that streams can be combined and you’ll start to understand why this cleans up the asynchronous nature of JavaScript.

The result is that we can write code that fills the stream and other code that says, “when a particular item comes in on the stream, let me know about it.” Basically, an embellished publish/subscribe design pattern.

From the description above, you should be able to see that Reactive programming can be used to implement Flux.

This is exactly what NgRx/Store does. It allows us to concentrate on writing Reducers while it focuses on managing the dispatcher, event handling, and the various repositories, or stores, our application might need.

As I’ve used NgRX/Store in my own applications, I’ve found that it further reduces the need for dependency injection and increases the testability of my code. The tradeoff is that there is a learning curve. But the time learning this new design pattern is well worth the effort.

Client Side Data
----------------

At some point, you are going to need to manage the data on the client side. There are several issue you might want to consider. But at the most fundamental layer, you will end up with data on the client side that looks like a relational data in a database. The only difference is that your data will be primarily JSON data. Eventually, you’ll want to join that data or filter it. How will you do that? One product you might consider is [Breeze](//www.getbreezenow.com/). It does a lot of stuff that makes your client side data look more like a database. It is worth a look

If you end up using NgRX/Store and RxJS, you may find that does everything you need. So that is another option. The advantage to using this method is you are no longer constrained by trying to make everything look like a relational database table even when it isn’t

The other option, of course, is to use both for what they are good at.

Conclusion
----------

Angular 2 brings a lot of new concepts. While it might be tempting to use coding patterns that seem more comfortable, I believe that the path that Angular 2 has chosen is the future of JavaScript specifically and the programming world in general.

Just about everything that is new falls into the general category of “Functional Programming” and much like the switch from procedural programming to object oriented programming, there are going to be people who are not able to wrap their heads around the concepts. How many guys ended up using C++ syntax to write C code? However, the productivity gains once we make the jump to this new way of thinking about our code will be well worth both the learning curve and the possible loss of some older programmers who can’t or won’t be retrained.

There is also a danger of not being able to retain good programmers because we are still stuck using design patterns that were popular in the past but have been superseded by ways the developer community at large considers “better.”
