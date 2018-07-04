---
title: Replacing an Element in an Array with RxJS
tags:
  - RxJS
url: 4509.html
id: 4509
comments: false
categories:
  - RxJS
date: 2017-11-21 06:30:34
---

It is not uncommon in our programming endeavors to need to replace one element in an array.  Using old school procedural programming, this would be relatively easy.  Loop through the elements, when we find the one we want to replace, change the value.  Basic for/next loop with a conditional statement. But when you move to a more functional way of programming as we need to do for NgRX, or are encouraged to do to make our code more testable, the problem becomes less straight forward. The initial solution you might try would be to just run `reduce()` against the array.  But if we do this, we still need to put that nasty conditional within our reducer function.  This is something we'd prefer to avoid.  Yes, it will work.  But it isn't Functional.  This problem has bothered me for months.  I've finally spent the time to figure out the solution. <figure>![](/uploads/2017/11/2017-11-21.jpg "Replacing an Element in an Array with RxJS")<figcaption>Photo credit: [Manchester Library](//www.flickr.com/photos/manchesterlibrary/2034771121/) via [Visualhunt](//visualhunt.com/re/1b8ae8) / [ CC BY-SA](//creativecommons.org/licenses/by-sa/2.0/)</figcaption></figure>

<!-- more --> 

Simple Problem
--------------

For the purposes of our discussion, we are going to assume that we have an array of integers, 1 through 5.  We want to change the value of 3 to 33.  If we were going to just extract the value and change it, we would use a filter.  But what we want to do here instead is split the array into two streams.  Elements that are 3 and elements that are not three.  You might reach for the filter function to do this. 

``` typescript
const array = [1, 2, 3, 4, 5]; 
const item = array.filter((x: number) => x === 3);
const notItem = array.filter((x: number) => x !== 3);
```

Merging Arrays?
---------------

But the problem you'll run into almost immediately is that now that we have the array split in two, how are we going to merge them back together again?  For this, we would need the `Observable.merge()` method.  But, arrays are not `Observables`. So, let's rethink this problem.  What if we turn the array into an observable?

Observable Arrays
-----------------

We can still use the `filter()` but now we can merge the results. 

``` typescript
const array = Observable.from([1, 2, 3, 4, 5]);
const item = array.filter((x: number) => x === 3);
const notItem = array.filter((x: number) => x !== 3);
const mergedList = Observable.merge(notItem, item.map((x: number) => 33);
```

Reconstitution
--------------

And now that our array, that is now an observable, is merged back together again, we can use `reduce()` to turn it back into an array. 

``` typescript
const reduced = 
  mergedList.reduce((acc: Array<number>, element: number): Array<number> =>
    return [...acc, element], []);
```
And subscribe() to get the return valued. 

``` typescript
reduced.subscribe((x: Array<number>) => /* do something with the array here */);
```

Out of Order
------------

But, we still have a problem.  Because we are working with an array, there is no timing to make sure the 33 is where the 3 was.  So, we end up with an array that has 33 at the end.  Maybe that's OK.  But there are times when we need to change the array without changing the order of the elements.  What do we do now?

Async to the Rescue
-------------------

It turns out that `Observable.from()` takes a second parameter that controls how the elements are handled.  If we pass in `async` for that parameter, the elements stay in order.

One pass Filter
---------------

Now that we have this all working, there is one final tweak we can make.  Rather than creating two different, but very similar filters, we can use the partition() method to achieve the same result in one pass. This, combined with array destructuring, allows us to simplify the code where our filter is, to 

``` typescript
const [item, notItem] = array.partition((x: number) => x === 3);
```

And now you have a Functional replacement of an element using RxJS.

Final Code
----------

Imports you'll need:

``` typescript
import { async } from 'rxjs/scheduler/async';
import { IntervalObservable } from 'rxjs/observable/IntervalObservable';
import { TimerObservable } from 'rxjs/observable/TimerObservable';
import { Scheduler } from 'rxjs/Scheduler';
import { Component } from '@angular/core';
import { Observable } from 'rxjs/Observable';
import 'rxjs/add/observable/interval';
import 'rxjs/add/observable/from';
import 'rxjs/add/operator/partition';
import 'rxjs/add/operator/map';
import 'rxjs/add/operator/reduce';
import 'rxjs/add/observable/merge';
```

Code:

``` typescript
const array = Observable.from([1, 2, 3, 4, 5], async);
const [item, notItem] = array.partition((x: number) => x ===3);
Observable.merge(notItem, item.map((x: number) => 33))
  .reduce((acc: Array<number>, element: number): Array<number> =>
    acc = [...acc, element]
  , [])
  .subscribe((x: Array<number>) => this.title += JSON.stringify(x));
```
