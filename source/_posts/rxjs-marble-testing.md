---
title: Real World RxJS Marble Testing Revealed
tags:
  - angular
  - Marble Testing
  - RxJS
url: 4351.html
id: 4351
categories:
  - Angular
  - RxJS
date: 2017-06-13 06:30:25
---

There must be some evil plot to keep this information a secret because whenever I search for how to use RxJS Marble Testing all I see is how to use it to write tests for RxJS.  Well I’ve finally “cracked the code.” 

In this article you’ll learn the basics of RxJS Marble Testing and how to apply Marble Testing to your own code.

<figure>![](/uploads/2017/06/2017-06-13.jpg "Real World RxJS Marble Testing Revealed") Photo via [Visualhunt](//visualhunt.com/re/9662e0)</figure>

<!-- more --> 

## It All Started When …

It all started about a month ago when I needed to write a unit test for code that updated the screen once every 15 seconds. Writing a test that would simulate the clock moving forward 15 seconds in old school JavaScript would have been pretty easy.  But I had written my code using RxJS instead of the old school `setTimeout()` method we’ve been using for this kind of feature in the past. 

Specifically, I had used `Observable.timer(0, 15000);` 

My first attempt to write tests for this were based on the assumption that, under the hood, `setTimeout()` was still being used. Just a simple matter of mocking the clock and advancing the timer. Right?

Wrong!

Then my initial search brought me to the NgRX 4 way of writing tests for Observables. Only, I didn’t find that out until I had written some code that didn’t work. 

Eventually, I found this thing called Marble Testing. But, as I’ve already mentioned, all the examples I’ve found for how to write test are for written test for the various methods available in the RxJS library. 

I’m guess, if you are still reading, I’ve described your journey as well.

## Assumptions

For the remainder of this article, I’m going to assume you know how to use RxJS. If you don’t there is already a lot of good information available on that topic that you should easily be able to find by using one of the search engines.

I’m also going to describe how to use marbles in your tests using Jasmine. I use Jasmine because that is the engine all the frameworks that I use end up using. There are other tools that may or may not be better. But I have no reason to leave what everyone else has already picked as a defacto standard.

## Marble Basics

Since there is a lot of detail on the basics of using Marbles, I only plan on summarizing what you need to know here.  For more details, check out [the documentation](//github.com/ReactiveX/rxjs/blob/master/doc/writing-marble-tests.md).

### Create Observables

First, you can create either hot observables or cold observables. To do this, you’ll need to create an instance of `TestScheduler` and then you call either `createHotObservable()` or `createColdObservable()` passing a string that defines what you want your observables to do. 

``` typescript
const testScheduler = new TestScheduler();
const hotObservable = testScheduler.createHotObservable(hotMarbleString);
const coldObservable = testScheduler.createColdObservable(coldMarbleString);
```

### Marble Syntax

*   `"-"` time: 10 "frames" of time passage.
*   `"|"` complete: The successful completion of an observable. This is the observable producer signaling `complete()`
*   `"#"` error: An error terminating the observable. This is the observable producer signaling `error()`
*   `"a"` any character: All other characters represent a value being emitted by the producer signaling `next()`
*   `"()"` sync groupings: When multiple events need to single in the same frame synchronously, parenthesis are used to group those events. You can group next values, a completion or an error in this manner. The position of the initial `(`determines the time at which its values are emitted.
*   `"^"` subscription point: (hot observables only) shows the point at which the tested observables will be subscribed to the hot observable. This is the "zero frame" for that observable, every frame before the `^` will be negative.

The most simple of observables using marbles would look like this: 

``` typescript
   'a|'
```

This would cause an observable event to fire right away and it would pass “a” as the data for the observable.  The observable would then end because the | comes next. 

In the case of the timer I was testing, I don’t need the data, I just need the “event” to fire so my code runs.

## Marbles as Mocks

So far, I probably haven’t told you anything that you couldn’t already figure out by doing a basic search.  But, the question remains, how do we use this marble stuff in our own test? 

And the answer that no one seems to be talking about is that you use marbles to mock out the real observable just like you might create a mock object to replace a real object in any other test. 

In the case of the timer problem above, what I needed to do was to make sure that `Observable.timer(0, 15000);` returns an observable that was created with a marble instead of an observable created with the timer.  In Jasmine, we do that with `spyOn()` 

``` typescript
testScheduler = new TestScheduler(null);
spyOn(Observable, 'timer')
  .and
  .returnValue(testScheduler.createHotObservable('---a|'));
```

The rest of your code really doesn’t care what kind of observable it is, it will do whatever it is it has been coded to do. 

The only thing you need to do to make sure the observable and subscribes do their thing is to make sure you call `flush()` on the `TestScheduler` instance prior to running an `expect()` in your Jasmine test.

## Other Uses

What I’ve shown so far takes care of my 15 second refresh issue. But now, what if I have an observable that expects data? For example, how would I write a test that uses an observable based on an AJAX request as a dependency? In Angular, this would be Http. 

This is actually very simple. The second parameter to either `createHotObservable()` or `createColdObservable()` is the data you want to send on to the subscribe when it hits the associated marble. You pass this in as an object literal. So, just to keep things simple. Say that when you hit the “a” marble, you want to pass the subscribe an object that has a first name and a last name. Your code might look something like this:

``` typescript
testScheduler
  .createHotObservable(
    '-a|',
    {a: {firstName: 'Dave', lastName: 'Bush'}}
  );
```

By using this type of marble mocking, you could not just create unit tests, but you could also create End to End tests that use marbles to return consistent data rather than hitting the back end.  Obviously, you would still need to write tests that ensure your back end is going to return the same type of data, but I see that as a separate issue from ensuring that the front end does everything it should do as a system.  Anyhow, it is an option.
