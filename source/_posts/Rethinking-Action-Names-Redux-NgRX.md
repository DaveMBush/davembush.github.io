---
title: Rethinking Action Names (Redux/NgRX)
date: 2021-05-29 06:52:47
tags:
  - angular
  - react
  - ngrx
  - redux
---

If you've been using some form of Redux, you are familiar with the basic flow of data through the Redux loop. Central to this flow are Actions, the messages that trigger code in our Reducer or Effect/Epic/Saga (depending on what flavor of Redux you are using).

The model allows us to disconnect our code so that it only cares that an action was triggered in some way.  That is, "when this action occurs, I will run this code."

Because of this, we can create an action that triggers multiple code blocks to run. Our only concern is that the code that gets triggered can't depend on each other.

In fact, much of the official literature encourages this practice.

And, this is where all our troubles begin.

<!-- more -->

## Current Recommendation

You see, the official literature around actions also suggest a naming convention that is tied to what we want to happen, rather than what just happened or something otherwise more generic.

For example, take the simple act of loading a list of employees from the server. To do this, we would typically create two actions.  The first would be `LoadEmployees` and the second would be `LoadEmployeeResults`. Both of these actions represent what we want to do, not what just happened.

Now, let's say we also want to display some sort of wait state and that we want to control it using Redux.  For that we would create a Reducer in our store.  Let's call it `Wait`. And for the purposes of this post, we will assume that `Wait` tracks wait state by incrementing and decrementing a counter.

But, we don't need to create new Actions for our Wait state because we can re-use the actions we've already created. When we fire `LoadEmployees` we can increment `Wait` and when we fire `LoadEmployeesResult` we can decrement the load.

## What's Wrong With That?

Do you see the problem here? We now have a reducer responding to an event that is not obviously tied to an action that has anything to do with the code that is getting executed. `Wait` is not a `LoadEmployees` thing although, you might reasonably say it is a `Load` thing.

And so how might we think about restructuring our code so that this makes more sense.

## A Change In Perspective

What if instead of naming actions what we wanted to do, we named them something more along the lines of what just happened.  That would make our `LoadEmployees` action `NeedEmployees` and our `LoadEmployeesResult` action, `EmployeesLoaded`.

Even these names don't get at the fact that Needing Employees doesn't mean we need to increment the wait state.

## Rethinking The Problem

One way we can think about this problem is to look at other messaging systems, like the windows messaging system. It's been a while since I've used the raw messages in Windows but I seem to recall a `WM_BUTTON` or `WM_BUTTON_CLICK` message that would get passed to my application when a button was clicked.  One message for every button in my system that had, as part of the payload, what button was clicked so my code could listen for that message for that button and do something because of it.

We could do something similar with the code above. Instead of tying the actions to particular code we want to run, our actions could be more generic.
What if we had two generic messages, `Load` and `LoadResult`. The payload for these messages could then have a `LoadType` property that defined WHAT we were loading so that we could Load Employees, Addresses, and Phone Numbers all using the same two actions and we could also use `Load` and `LoadResult` to increment and decrement our wait state.

This would work except we still have a problem.

## That's Not How It Was Designed

Most of the tools we currently have in place to reduce the boiler plate code we need to write are based on a one-to-one relationship between actions and the code that gets run. This isn't to say that we couldn't adapt them but doing so would be more trouble than it is worth because we would be working against the intended design.

And so, I would suggest to you that the advice the current literature gives us about using Actions to trigger multiple blocks of code is wrong. Since Actions are primarily used as loosely coupled method calls, using them in unrelated places violates the Single Responsibility Principle and makes the code hard to follow.

I've seen one code base where the code that gets triggered has absolutely nothing to do with the Action that triggers it. Not something in the simple case of LoadEmployee triggering Wait, but in more obscure relationship like `LoadCandyBars` triggering `LoadSteakDinner`.  The result is that the code becomes extremely difficult to follow. This is one of the biggest causes of bugs in this system.

## Maybe This Will Work

For our discussion about the Wait state, I would suggest that we create a separate set of Actions for our Wait state, possibly WaitStart() and WaitEnd() that get fired when we fire EmployeeLoad() and EmployeeLoadResult(). You could wrap these calls in a function or method to ensure they always get called together, but the actions themselves need to be unique to make the code easier to follow.

Currently, this is my recommended implementation. While not ideal, it gets the job done and makes maintaining the code simple and straight forward even if I do have to duplicate more Actions that I would like to.
