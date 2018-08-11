---
title: Functional Reactive Angular Revealed
tags:
  - angular
  - NgRX
  - RxJS
url: 4341.html
id: 4341
categories:
  - Angular
date: 2017-05-30 06:30:46
---

Over the last month or so, I’ve been presenting the basics of [how to use NgRX/Store with Angular](/tags/ngrx/).  In the past, I’ve praised the virtues of [Reactive Forms](/tags/reactive-forms/), also known as [Model Driven Forms](/tags/model-driven/).  These along with RxJS make up the pillars of Functional Reactive Angular Programming. What is sad is that this reality is lost on so much of the Angular community.  When I listen to podcast where they talk about any of these concepts individually, Function Reactive Programming (FRP) is barely, if ever, mentioned. But the scary thing is this, there are many people who are going to use the new Angular the way they used the old Angular and they will completely miss the main advantages.  They may even jump from Angular to React or (even) Aurelia.  And that’s just picking on the most recent frameworks.  Some will want to go back to Egypt and decide jQuery is a good choice! Functional Reactive Programming is not just a hot new model.  It solves a lot of problems. <figure>![](/uploads/2017/05/2017-05-30.png "Functional Reactive Angular Revealed") Photo via [VisualHunt.com](//visualhunt.com/re/b10788)</figure>

<!-- more --> 

Object-Oriented Failure
-----------------------

Several years ago, I wrote an article called, “[Object-Oriented Programming Has Failed Us](/object-oriented-programming-has-failed-us/)”.  In the article, I put forth the reality that most people are unable to think in object-oriented terms.  Humans think sequentially and chunk down.  Object-Oriented Programming requires us to think holistically, frequently in parallel, and chunk up. So, lets define some terms.  Chunking down is the act of finding differences between things.  This is why we discriminate.  At times, it is useful to find differences.  If you are a microbiologist, you want to do this so you know you are working with one strain of virus vs some other strain. As you might have guessed, Chunking Up, is the exact opposite.  We look for commonalities.  Given two different things, what is the same between them? The problem is, that’s not how most of us naturally process the world. There are other problems with Object-Oriented Programming.

### Hard to Reason About

Back when I was teaching for a training company, I was explaining some concept of Object-Oriented Programming, probably Virtual Functions, to a student that came from a procedural programming world.  His comment was, “this is going to make the code really hard to debug!” to which I responded, “not if you step through with the debugger.”  But, the truth is, he was right.  Debugging Object-Oriented code is difficult because the code is hard to read and reason about.  Looking at any one class, am ever sure that I’m seeing the whole picture? Think about this.  When you write a class, and have a member variable, how long is it before you’ve forgotten the fact that the variable exists and is implicitly a parameter that is being passed to your function.  Not long!  And this means that you can never really be sure that when you go to use that function, the state of the object will always be the same. This makes the code incredibly hard to test.  I’m not even talking about using Test Driven Development.  Just any kind of test that have way ensures that the code you’ve written does what you think it does and doesn’t do what you don’t think it does.

### Single Responsibility

One of the rules for Object-Oriented Programming is that we should follow the “Single Responsibility Principle” I ask you, how far do we chunk down to ensure we are only doing one thing in our class?  In fact, many of the principles we have developed are trying to put fences around the inherent problems with programming in an Object-Oriented mindset.

Procedural Programming
----------------------

The benefit of Procedural Programming is that I at least knew what parameters I was always going to get.  The disadvantage is that it was still incredibly hard to test because my function could call other function that could call yet other functions.  This meant that I could only really test the functions that were at the end of the hierarchy and really had no good way of mocking out child functions.  While testing is hard in an Object-Oriented world, it is nearly impossible in a procedural world.

What If
=======

But, what if there were a way to write code that solved most of these problems.  A way that better mirrored how we thought, that is easier to reason about, that allows you to chunk down – that forced you to chunk down.  A way of coding that was so easy to test, that you frequently didn’t even need to write the test.  This is the advantage of Functional Programming generally, and the reason you want to use the combination of NgRX/Store, Reactive Forms, and RxJS in the bulk of your Angular code.

How To
------

### Basics

You’ll remember from our discussion of NgRX that we setup a reducer to return a new state object for a particular entity in our store.  You’ll also remember that we are able to create an entity that has child entities and that we can subscribe to any entity using code that looks something like this: `store.select(x => x.entityName)` or `store.select(x => x.entityName.subEntity)` If you need to, you can read my previous articles that I’ve already referred to. I normally setup an entity in my store for each screen, then for data I am just displaying I subscribe in the template using `{ {(observerThing | async)?.variableThing}}`. The `(observerThing | async)` is the same as the code we would normally write in TypeScript: `observerThing.subscribe(x => x);` `async` does the `subscribe` and returns `x`.  The `?` ensures that we don’t attempt to go after `.variableThing` if `x` is `null` or `undefined`. This is all pretty basic stuff. But what about working with forms.

### Forms

I’ve found that the best way to work with forms is to create a structure in my entity that maps directly to my form.  So, if I have a form with firstName, lastName, and birthDate, I’ll define my form in my template with a form group using firstName, lastName, and birthDate as formControlName values. Next, in my TypeScript code, I’ll subscribe to the form group’s valueChanges property.  Any time my form changes, the subscribe dispatches the changes to the reducer for my entity and updates the store. `this.myFormGroup.valueChanges.subscribe(x => store.dispatch(/* action thing goes here */))` Similarly, I can setup a `subscribe` on my entity and any time the data in the entity changes, I can update the form group. There is a small little trick you need to know about here.  I already showed you how to update the form group using `patchValue()` [here](/angular2-model-driven-forms-are-superior/).  But because we were not dealing with a fully Functional Reactive programming model, I left out a part you’ll need here. When you update your code using `patchValue()`, the first parameter will still be the data you want to change.  That is, the values from the store’s entity.  But for the second parameter, you’ll need to pass in `{emitEvent: false}`, otherwise, you’ll end up with an infinite loop.  Your form will cause your reducer to change and your store will cause your form to change.  What that second parameter is saying is that we don’t want any of the change events to fire because we’ve updated the form. You may also want to consider writing code in your reducer that only returns a new state object if no data has changed.

### RxJS

Now, nothing about anything I’ve written in the “how to” is particularly Functional, but it is Reactive.  By virtue of the subscribes, it is reacting to state change and reacting to changes in the form.  What makes our code Functional, is that we make use of RxJS, a Reactive library, to process the data.  If you’ve already been using the Http object in Angular, you’ve already been using RxJS. There is a great [tutorial for learning RxJS](//reactivex.io/learnrx/) that the guys at NetFlix have put together.  I’m not even going to attempt to teach RxJS here.  They’ve got the best material and it is what helped me wrap my head around the basic concepts. What I do want to point out here is what makes Functional programming Functional so that you end up using RxJS correctly.  While these are not hard and fast rules, I would encourage you to break these rules only after you can’t find any other way:

1.  The output of one function becomes the input for the next function.
2.  A function never causes a side effect (this is why NgRX has Effects)
3.  The same parameters in will always produce the same data out.
4.  Avoid conditionals and use .filter(), .case() etc instead.
5.  Ideally the cyclomatic complexity of a function should be 1

By following these rules, you will find that most of the code you write doesn’t need to have any test.  Why would you ever test a function with a cyclomatic complexity of 1, or code that has no conditions?

The Right Tool for the Job
--------------------------

While I would love to be able to use Functional programming everywhere, I recognize that it isn’t always the best tool for the job.  For example, at least with Angular, there isn’t a good way of writing our components and pages in a strictly Functional way.  However, if you use what I’ve illustrated here, you’ll find that even though the structure of your components are Object-Oriented, much of the code within the component is quite functional.
