---
title: Reasons to use RxJS Today
tags:
  - javascript
  - rxjs. angular 2
url: 4117.html
id: 4117
categories:
  - Angular
date: 2016-12-13 07:30:00
---

If you’ve started looking at Angular 2, one of the things that you’ll notice is that [RxJS](//github.com/Reactive-Extensions/RxJS) has gotten a bit of a toe hold in the framework. This becomes apparent the first time you try to access data. Gone is the $http service that returns a promise. Instead, we now have a service that returns an Observable. Now, writing the code to access the server is arguably easy to learn. But, as you travel down the rabbit hole that is Angular 2, you realize that RxJS shows up in places as disperse as [NgRX/Store](//github.com/ngrx/store), handling events, and as we’ve already mentioned, AJAX calls.

Because it shows up in so many places, this new API is set to be the next thing we will need to learn to be effective JavaScript programmers. But, should we?

<figure>![](/uploads/2016/12/image.png "Reasons to use RxJS Today")<figcaption>Photo credit: [Dace Kiršpile](//www.flickr.com/photos/91035846@N05/9374805577/) via [Visualhunt](//visualhunt.com) / [CC BY](//creativecommons.org/licenses/by/2.0/)</figcaption></figure>

<!-- more -->

A Story
-------

As I first started to dig into using RxJS, I found myself being more confused than I expected. And yet, I was convinced that this new way of thinking about my code would be well worth the effort.

It reminds me of the days when we switched from Procedural Programming (PP) to Object Oriented Programming (OOP). Yes, I’ve been coding that long.

Somewhere around the third year of my career, C++ finally became popular enough that both Borland and Microsoft produced a C++ compiler and framework for developing Windows applications. I remember falling in love with it. Maybe I hadn’t been using procedural programming long enough to make switching difficult. Maybe I just thought about code as Objects anyhow. But, whatever the reason, I couldn’t figure out why some of my peers had such a hard time grasping the concepts.

As I was working on learning RxJS, I found myself feeling like I was either too old to learn a new trick, or I was just getting dumb in my old age. Why is it so hard? Fortunately, I found a site that helped me understand RxJS specifically and Reactive Functional programming in general. I am no longer in the class of “old farts who can’t learn new stuff.” And now that I understand, everything is starting to look for Reactive.

What Is RxJS?
-------------

If you’ve ready anything about RxJS or any of the ReactiveX libraries that are available for other languages, you’ll find that most of them start off by saying that RxJS “makes everything a stream.” Which, while that may be technically true, isn’t very helpful. What would it look like if we redefine that? The best way to think about RxJS is that it makes everything a special type of collection called an Observable.

### Arrays

In JavaScript, you are already deal with a special type of collection called an Array.

Now, let me lead you though the thought process you’ll need to travel down to see how RxJS is going to be helpful for you.

For now, let’s just assume everything is an array. This will make the jump easier.

In pure JavaScript, you are typically writing code that either looks like this: (old school)

``` javascript
var someArray = [...data here...];
for(var i = 0;i < array.length;i++) {
    var a = someArray[i];
   ... do something with the data ...
}
```

Or using newer syntax:

``` javascript
var someArray = [...data here...];
someArray.forEach(function(a) {
 ...do something with the data...
});
```

[Array Extras](//code.tutsplus.com/tutorials/what-they-didnt-tell-you-about-es5s-array-extras--net-28263) takes some of the code we typically write in these loops and standardizes them so we can reduce the amount of code we need to write, while still achieving the same goals.

For example, you might loop through an array to generate a new array. For that you would use map(). Or you might loop through an array to filter out elements. That would be filter().

The great thing about this Array Extra functions is that they each return a new array. This means you don’t impact the original array and you can chain these functions together:

``` javascript
var someArray = [...data here...];
var transformedAndFiltered = someArray
   .filter(function(item){...filter code...})
   .map(function(item){...tranform code...});
``` javascript

`someArray` still has the original data and transformedAndFiltered has the new data.

### Events

Now, imagine we create a system where we have arrays that never end. Instead of using the old school for/next loop, we would be left with only being able to use a forEach, or one of the Array Extras function. Which would be fine. The function would just wait until the next element showed up or it got some sort of signal that there wouldn’t be any more elements.

This is exactly what RxJS does. So, now we can listen for events as though they were arrays.

``` javascript
Observable.fromEvent(element,'click')
   .subscribe(function(x){...do something in response...});
```

Pretty cool, right? If you need to, you can place a filter prior to the subscribe so that you only get events that meet the exact condition. In fact, if any of the functions you pass as parameters have conditional logic in them, this is a good indication that you are not yet thinking in a Reactive way.

### Asynchronous Code

Now, as I’ve already mentioned, RxJS also lets you deal with asynchronous code as though it is an array. This works pretty much as you would expect on the surface, but there are some added advantages to using RxJS to deal with asynchronous calls. The most significant of them being that you can achieve better flow of control.

If you’ve ever dealt with needing to make multiple calls to the server for data prior to allowing the user to use the screen, you’ll appreciate that you can make all the calls at once and only continue once all of them have completed. Or, you could make the calls one after the other, if you have calls that depend on each other. Yes, promises do this as well, but the Reactive method is a bit cleaner.

The other way RxJS improves on promises is that you can cancel a request instead of just letting it run even though you no longer want the data. This means, you won’t be tying up bandwidth waiting for data to come back when you want newer data instead.

Finally, using RxJS, you can easily retry a failing request, as well as deal with failing request using the .retry() and catch() functions.

Functional Matters
------------------

Now, all of this is great. But the side effect of the fact that RxJS is both Functional and Reactive is that it not only reduces the amount of code that you need to write, but it also produces code that is less prone to errors and easier to test.

Let me explain.

We’ve already shown above that all the RxJS functions return new Observables. I kind of brushed over this. But this is a big deal. Because the original data in the Observable is never modified, you never run the risk of modifying the data that some other part of your code is expecting to never be modified. This is even more important when you are working on a larger project with multiple developers. This means all your assumptions about your data will remain true when your team commits to using functional paradigms throughout your code base.

But this effect doesn’t just impact the observable code base. What I’ve found is that the more code you write that is functional and obeys the immutable rules, the easier the rest of your code is to test. Specifically, I find myself using less dependency injection simply because I am using immutable objects. Because I can depend on the code not having any side effects, my functional code is easier to test and my none functional, more object-oriented code, is easier to test because it has fewer dependencies.

Conclusion
----------

The most difficult thing about using RxJS or any other Reactive Functional library is the fact that it requires you to think a little bit differently about the problems you are solving. But the advantages far outweigh the learning curve. Don’t be like so many of the script kiddies on the web who complain every time they must learn something new. Step up to the challenge. Your customers will thank you. The guy who maintains your code in the future will thank you. Your future self will thank you.
