---
title: NgRX Effects Design Patterns
url: 4470.html
id: 4470
comments: false
categories:
  - none
date: 2017-10-17 06:30:03
tags:
---

Since I don't seem to have a blogs worth of any one topic to write about, I thought maybe something more along the lines of a cluster of small tips and tricks might work.  Since I've been writing my Angular book over the last couple of weeks, there hasn't been much new that I've discovered worth sharing.  But there are a few small discoveries I've made.  This week, they center around NgRX Effects. <figure>![](/uploads/2017/10/2017.10.17.png "NgRX Effects Design Patterns") Photo via [Visual hunt](//visualhunt.com/re/79a90d)</figure>

<!-- more --> 

Returning No Actions
--------------------

There have been times in the past where I didn't want to return an Action from my Effect.  Not knowing any better, I just returned the Object Literal, `{type: 'noop'}`.  But there is a much better way. BTW, did you know that unless you tell NgRX otherwise, all @Effects must return an Action? So, what is the better way?  It turns out that the guys who wrote NgRX already thought of the possibility that you wouldn't want to return an action from an @Effect.  All you need to do is to pass the object literal `{dispatch: false}` to the @Effect() decoractor. 

``` typescript
@Effect({dispatch: false})
member$: Observable<{}> = 
  this.actions$ ...
```

Like I said, I needed this because I didn't yet know about some of the other things I'm going to mention below.  My experience says, if you need this, you are probably doing something wrong.

Strong Typing
-------------

If you've read any of my other post about Angular, you'll know I'm a very strong proponent of strongly typing everything.  Nowhere is this more useful than when creating the Effect chain.  By typing everything, I can easily tell that my code is written correctly.  Even while writing my book on how to get started with Angular, it has helped me figure out that I'm missing a closing bracket or parenthesis.  Not to mention it has told me exactly what block of code I had wrong.  I've probably saved hours of debugging time with this one simple enhancement to my coding practice.

Chaining Observables
--------------------

I'm almost ashamed to mention this next one.  But, I never really caught on that `switchMap()` was a silent subscriber.  Oh! So many places you can use this. If you've been using Effects, you've probably seen code that looks like this:

``` typescript
@Effect()
get$:Observable<Edit.Update> =
  this.actions$
    .ofType(Edit.GET)
    .switchMap((action: Edit.Get): Observable<{} |
      ReadonlyArray<Contact>> =>
        this.contactsService.get(``action.id)
    .map((x: ReadonlyArray<EditForm>):``Edit.Update =>
      newEdit.Update(x[0]));`
```

But, do you know why it works? That `switchMap()` bit expects that you'll return an observable.  It subscribes to it and passes the data inside the observable to the `map()` that follows.  Kind of like how the `async` pipe works in our templates. The cool thing about this is that you can chain subscriptions together using this.  It isn't just some cleaver way of getting the results of a service into your final action that you will return.  I've used `switchMap()` to get data from the store and pass that on to a `switchMap()` that call my service with that data. The only thing you need to watch out for is that your observable is either a cold observable or that you call `.first()` so you it becomes a cold observable.  Otherwise, you'll end up in an infinite loop.  Ask me how I know :).

Grouping Observables
--------------------

Chaining observables together using `switchMap()` works well when the output only needs to be used by the next function in the chain.  But what about when you need to use the results from two different observables at once.  There are a couple of different ways you might do this. One function you might consider using is `withLatestFrom()`. 

``` typescript
firstObservable
  .withLatestFrom(secondObservable, ([firstResult, secondResult]) => 
  // your code here)
```

or if you have more than two you need to combine, you can use `combineLatest()` Another way you might consider combining them is with `forkJoin()` although that might be the last tool you reach for.

Single Responsibility
---------------------

Another mistake I've been making is trying to make my Effect do too much.  If I restrict my effect to only doing one thing at a time and always returning one or more actions, they become much easier to manage.  Maybe this has only be my problem.  But once I saw what switchMap() could do for me, the light bulb went on in my brain.

Returning Multiple Actions
--------------------------

And speaking of returning one or more actions, it is possible to return multiple actions from an Effect using.

``` typescript
.mergeMap(
  ( result: string[] ) => {
    return Observable.from([ 
      new ActionOne( result ), 
      new ActionTwo() 
    ]);
  }
)
```
