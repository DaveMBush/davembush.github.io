---
title: rxjs-conditional-logic
date: 2020-11-05 08:01:10
tags:
  - RxJS
---

The temptation, when using RxJS is to include conditional logic inside your map, tap, or subscribe blocks.  But there is a much better way to deal with conditional logic that will make your code much easier to read and reason about. It also should make your code easier to test. But of course, once you have code that is easy to test, you probably no longer need to test it.

## Filter

The easiest, most straight forward way to handle conditional logic is by using the `filter()` operator.

``` typescript
streamThing.pipe(
    filter(x => condition),
    // additional code here
  ).subscribe();
```

This code shouldn't look so strange.  It pulls your logic out from within whatever additional logic you have and also reduces your need for multi-line map, tap or subscribe blocks.

## Partition

On the other hand, you may need an if/else block.  This is what `partition` is for.  The partition function is considered an Observable creation function because it takes one stream and returns two more.

The `partition` function works similar to the `filter()` operator.  You pass in the stream you want to split and the predicate for the truthy evaluation and the function returns an array where element 0 is the true Observable and element 1 is the false Observable.

Where things can get tricky with this operator is that each stream inherits the stream it was based on.  If you subscribe to either, it will trigger whatever action created the original Observable.  That is, if your original Observable is based on a call to the server and you subscribe to both of the new Observables that `partition()` created, you call to the server will happen twice.

Therefore, you should `share()` the original Observable before you split it with `partition()`.  This will prevent the original Observable creations from getting called twice.

Once you have the two Observables, you'll probably want to merge them back together at some point and subscribe to both at the same time.  There are multiple ways you might consider doing this, but you will use `merge()` most often.

``` typescript
let [trueObs, falseObs] =
  partition(
    originalObs
      .pipe(share()),
    (x) => /* x true condition */)
merge(trueObs, falseObs)
  .subscribe();
```

## Case Statements?

The closest operator we have to a being able to perform case statements is `groupBy()`. But this is not typically not what you want because you have no idea where the result will show up in the results.

No, the best you can do is to start with a base `Observeable` and use a filter against it for each of the conditions and then merge all the results back together.

In fact, you may find that using two filters makes your code easier to reason about than if you were to use `partition()`.
