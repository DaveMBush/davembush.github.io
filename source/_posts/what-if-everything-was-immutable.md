---
title: What if Everything Was Immutable?
tags:
  - angular
  - immutable
  - javascript
url: 4168.html
id: 4168
categories:
  - Angular
date: 2017-01-10 07:30:00
---

The first time a programmer who was trained in the classical procedural/object oriented history is confronted with the concept of making everything immutable, the first question that comes to mind is, “won’t that make my application slow?”  This is because of how most programmers have been trained.  Making everything immutable generally means that we must copy a lot of memory from one place to another.  Moving memory around is generally considered slow.

And so, most programmers dismiss the whole idea as crazy talk.  But is it really all that crazy?

<figure>![](/uploads/2016/12/image-4.png "What if Everything Was Immutable?")<figcaption>Photo credit: [Paul Stevenson](//www.flickr.com/photos/pss/354177349/) via [Visualhunt](//visualhunt.com) / [CC BY](//creativecommons.org/licenses/by/2.0/)</figcaption></figure>

<!-- more -->

Two Paths
---------

So, let’s look at some computer history.  If you’ve been in this industry any length of time, you’ve probably heard the name Turing and you know the Turing machine has something to do with programming.  But you may not know how it impacts your day to day programming life.

In short, Turing is the guy we can point to as the father of modern computing, complete with the fact that we generally process everything sequentially.  It is this sequential processing that causes us to have instructions working on memory and it has served us well.

But there is another branch of computer science that was made popular by Alonzo Church that is based on Lambda Calculus.  This branch of computer science is where the bulk of Functional Programming can be traced back to.  In this branch, everything is a function.  Any parameters you pass in cannot have their state changed and the value that is returned is a new value.

The main benefits to this newer functional style are:

*   If I pass in the same parameters, I’ll always get back the same value.
*   State is never mutated accidentally because state is never mutated.
*   Functions never have an unpredictable side effect.

Just think of how many bugs you wouldn’t have introduced into your code had this been true when you wrote it.

Convergence
-----------

Recently, functional programming has become more popular and the two branches of computer science have started to merge.  Taking the best of both worlds and using what makes sense from each.  But, we’ve already had at least one immutable object in most of our object-oriented languages.  The String object.  So, what does String give us that we use nearly every day?

Immutable Strings
-----------------

### No Side Effects

As we’ve already mentioned, that fact that something is immutable means it can’t be changed.  So, when you pass a string as a parameter, even though it is an object, the string won’t change if the function changes the string.  This is good news because the calling function doesn’t have to protect itself against unintended consequences.

### Memory Efficiency

The other thing that happens with immutable object is that every string representation only exist once.  So, we never duplicate memory.

What do I mean by this? Well, take this example from C#

``` javascript
var string1 = "abc";
var string2 = "abc";

if (string1 == string2) {
  writeLn('is equal');
}
```

### Testing Equality

`string1` and `string2` point to the same object “abc” not two different instances of “abc”.  This is why we can write `string1 == string2`.  Unfortunately, the == operator is overloaded so we are still doing string comparison.  

But if you want efficiency, you can use the [Intern()](/net-string-pool-not-just-for-the-compiler/) method and ReferenceEquals() to bypass this overload and use pointer comparison.  This is a good thing because we test for string equality and inequality so often the performance penalty we incur from making strings immutable is more than offset by the performance gains we get testing for equality.

Make Everything Immutable
-------------------------

If you think about the code you typically write, how much of the time would you benefit from every object in your code working in a similar way to strings? Wouldn’t it be a great thing if you knew that when you passed an object into a function, that function was not going to change the object at all?  Can you think of times that might have prevented a bug? Think of the memory optimizations you would benefit from.

And how much easier would it be to test for equality if all you had to test was a memory pointer?  Not only would it be fast, but it would be reliable and a lot less work to code.

Immutability is Hard – or is it?
--------------------------------

So now that you know what the benefits are, you are probably thinking, “Yes, but coding immutability into a language that doesn’t support it natively is a lot of work.  It is so much easier to write code the way I always have, even if it isn’t quite a safe.” Yes, I feel your pain.  But there are libraries for that.  Say you could get all the benefits of immutability with very little programming or performance cost?  Would you be interested? I know I was.  Just having the benefits of immutable data with the performance was benefit enough for me.  But then I was introduced to [immutable.j](//facebook.github.io/immutable-js/)s and I realized that I could have all of the benefits with a lot less performance cost than I was expecting because the library uses data structures that allow us to manipulate pointers rather than raw data.  The result is that only pointers to data that have actually changed change.  Nearly everything else stays as it was and adding items to List instead of arrays and Maps ends up being a lot more efficient.

Impact on Angular2
------------------

By using immutable object in Angular2 along with libraries like [NgRX/Store](//github.com/ngrx/store) you can see some major performance increases because the view will be able to determine when it needs to change based on simple object pointer comparison rather than checking entire objects.  For compute-intensive tasks, this is going to be a huge benefit.

But it is also well worth learning how to use this before you ever need it simply because it will be a new way of thinking about your project that may take some time to get accustomed to.
