---
title: Fixing Single Responsibility
date: 2020-10-04 13:58:05
tags:
  - Single Responsibility
  - Architecture
---

The Single Responsibility principle is a well-known, Object Oriented principle that states that we should narrow the scope of the code in our Module/Class/Function so that is it only responsible for one thing.

By doing so, this reduces the size of our code that needs to be tested.

Linting rules can generally help enforce this rule by making sure your Class file isn't too long, and your functions are not too complex. But there are other ways of violating this principle that linting rules can not pick up.

<!-- more -->

For example, in the world of Entity Framework, there has been a design pattern that has us putting our code in distinct layers.

- Controller
- Business Logic
- Repository
- Data Access

In the world of Angular, you may be using NgRX and OnPush notification (if you aren't you should). Here are the layers you should see in an Angular project:

- Component
- Component-Service
- NgRX
  - Actions
  - Reducers
  - Effects
  - Selectors

In both of these architectures, you can completely jumble your code by putting it in the wrong place.

You see, Single Responsibility is not only about "doing one thing" but it is also about understanding WHERE the code goes so that each layer of the code is only responsible for one thing.

## A Place for Everything ...

Both of these architectures have a definition of where everything goes. I continue to see, in both cases, code going in the wrong location.

### Entity Framework Example

A couple of examples.  The Repository layer is where we retrieve our data. This might be from the database and often is, but it might also be from a service of some sort.

The Business Logic layer is where we perform actions on that data to do some sort of meaningful work.

Clear?

#### Repository or Business Logic

Well, I guess it isn't because what I often see is the conflation of the two so that, most often, you end up with Business logic inside the Repository.

Unfortunately, there is an easy rule to follow that eliminates most of this.

The only thing your repository should return is an IQueryable of the type represented by the Repository name.  If your repository is for accessing a particular table, it should return an IQueryable of that table's Model and ONLY IQueryables for that Model.  Not some of one Model and some of another.

> Yes, I know, technically speaking, the Repository is both dead and could also return IEnumerable, but I find that adds to the problem of putting Business Logic inside the Repository.  So, I'm going to assert that IF you are going to use the Repository pattern, it really should ONLY return IQueryable.

In a typical Entity Framework application, your goal is to not actually retrieve data until you are in the Business Logic layer.  By returning IQueryable, you prevent any data retrieval from occurring and you also give yourself and your team the added benefit of being able to use multiple methods in your repository from the business logic layer by combining them all into one query.

Code reuse and a possible performance gain.  What could be better?

#### Projects

The other place I see issues with the Single Responsibility principle while using Entity Framework is when try to cram multiple classes for a layer into one project.  A good dose of Domain Driven Design would go a long way to helping this but here again, we need to consider what we are trying to do and the impact we are having on future coding efforts.

On really large projects, you can particularly end up with migration headaches because you've managed to combine everything into one Context.  If you were to break your models into multiple context, you could avoid many of these conflicts because they would each have their own migration history.

Similarly, at the controller level, you should consider how much sense it makes to add yet another end point to an existing class.  Or maybe it makes more sense to start a new class.

### Angular

#### Components

Angular components are pretty easy to create and, most of the time use code that has already been tested.  It is the logic of our components that is hard to test.  To separate the two, I suggest using a Component-Service leaving the only logic that is in the component.ts file logic that is specific to the presentation.

Think of it this way.  If you had to replace the presentation layer?  How much of the code would you have to duplicate or rewrite?  If you push that code down a layer, would that simplify things for you?

#### NgRX

One of the more difficult concepts for most people to wrap their heads around fully is the concept of NgRX.  With in this, where do I message my data?

The temptation is to message the data in the component or the component-service.  But a better location is in a function that a selector calls.  By doing this, you only end up messaging the data IF the underlying data changes and now your component and component service become ways of displaying what's in your store.

In fact, you may find that your component service has nothing to do because all the work has been done in the selector.

## The Real Problem

Given these examples, you'd expect me now to dive into how to really work these patterns.  But, I've been intentionally vague about the solutions because the problem isn't really about where stuff lives or how closely we follow the Single Responsibility principle.

The problem is that too many of us are hackers and not learners.

I made the comment to a friend of mine recently that the reason I don't believe a team should have options on architecture is because, largely the people on the team "learn" by copy and pasting existing code. Even if it is wrong.

And so, the reason we have issues with Single Responsibility at all is because, as an industry, we believe we can just dump people into a new language or a framework and they'll "pick it up."  Managers need to get beyond, "well it works" to "can this be maintained?"

To do this they need to do several things up front:

First, make sure their developers understand the patterns and practices they are using. This may mean sending them to training. This may mean pairing them with someone who you know already knows.

Second, you need a watchdog on your team to ensure that patterns are followed closely. In my experience, this is not something that comes naturally to programmers.

I've frequently said that it takes most programmers 5 years just to get to a point where they've stopped learning how to program and see programming as more than just learning the syntax of a language.  It takes another 5 years before they begin to see the advantages of architecture, if they see it at all. It could take another 10 years past that before they are able to mentor and train others.
