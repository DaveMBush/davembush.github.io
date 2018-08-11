---
title: Angular Observable Secrets Revealed
tags:
  - angular
  - observables
  - RxJS
url: 4412.html
id: 4412
categories:
  - Angular
  - RxJS
date: 2017-08-08 06:30:11
---

If you’ve been programming JavaScript based applications for any length of time, you’ve probably already made the progression from callback hell to promises, but just to recap.  Whenever we make any kind of asynchronous call in JavaScript, we need to provide a callback function to the call so that, when the call completes, the function can be called with any resulting data.  Function calls you may typically make that need this kind of feature are `setTimeout()`, `setInterval(),` and AJAX calls using the various libraries that support this. The problem with using callbacks is that you can end up with “Callback Hell” where you have callbacks inside of other callbacks.  Our code becomes messy and difficult to reason about. To try to flatten this situation out, promises were created.  Instead of creating a callback function and passing it into the asynchronous function, the asynchronous function returns a promise that has a function we can pass our function into.  This function can return yet another promise.  The result is that instead of having nested callbacks, all of our callbacks live at the same level. However, in the process, we lost the ability to cancel an asynchronous function using callbacks.  Most of the time, this was not a huge concern, but in the case of AJAX calls, we did end up making more request than we really needed to.  Most people never even recognized this as an issue.  But if you go and take a look at some of your older code, you will see that you have several places where the code would work more efficiently if you were able to cancel a call that was being superseded by a new call. Meanwhile, some additional functions were added to JavaScript Arrays.  Maybe you’ve seen some of them?  map(), reduce(), and filter() are three of the more common functions. What?  You haven’t seen these?  If you have and you know how they work, you can skip this next section.  But, if you haven’t, pay careful attention because this next section is critical to understanding how Observables work. <figure>![](/uploads/2017/08/2017-08-08.jpg "Angular Observable Secrets Revealed") Photo via [VisualHunt.com](//visualhunt.com/re/4ba464)</figure>

<!-- more --> 

Array Functions
---------------

### map()

Let’s say you have a list of objects that you need to transform into another form.  In the example below, we want to transform our list of objects into a list that can be used in a dropdown list using fullName for the display and id for the value. Without using `map()`, your code might look something like this: The thing is, we do most of the code from `newArray = []` on down over and over again.  It is only the code in the `push()` that changes. What if we were to make the code into a function?  That’s what `map()` does.  It takes a function as a parameter that takes an item as a parameter.  Inside the function, we use that item to specify how we want to transform the item and the whole map() function returns the new array. The code above, turns into this.

### filter()

Now, let’s say that for some reason, you only want to include items in the new array that include a last name that starts with ‘B’. Our old style code would look something like this: And once again, this is code we tend to write quite a bit.  So, what if we had a function that did this for us? This is exactly what the `filter()` function is for.  So, rewriting the code above using `filter()` would look like this.

### Chaining

Once again, you can see that we passed in a function that takes the current item as a parameter.  The function returns true or false.  If it returns true, the item gets included in the new array. What if we want to filter AND transform the data? The temptation for programmers new to this model is to use the map and push the item into an array that was declared outside of the map. But that really isn’t all that much better than if we were just using a for/next loop like we’ve been doing.  Old habits die hard. Instead, we can take advantage of function chaining. What this allows us to do is to filter and then map. So much cleaner.

### reduce()

The final useful function we have available to us for dealing with common array loops is reduce().  reduce() allows to loop through an array and accumulate the items in an array into another array, an object, or a value. The reduce() function take two parameters.  The first parameter is a function.  The second parameter is the starting value for the accumulator. The function that we pass in takes three parameters.  The current value of the accumulator, the current item, and the current item index.  Most people only use the first two parameters in their function.  The function returns the new accumulator value that then gets passed into the next call to the function. So, a really simple example would be, given an array of numbers, add them all up. I've also used this to turn an array of name/value pairs into an object where the properties are the name and the values are the values that were paired with the names.

Events as Arrays
----------------

Now, imagine that events that fire are part of one long continuous array.  An array that never ends. If this were true listening to events would be as familiar as processing an array. This is all an observable is.  It treats everything as though it were an array, adds several other functions that give us even greater functionality, and several functions that allow us to deal with the fact that events are not only sequential, but also time based. And because events aren’t really arrays, we call this series of items a “stream.”  So, when you read about “streams” while working with Observables think, “list of items.”

### Button Click

For example, let’s say you have a button on your screen and you want to know when it is pressed.  Let’s say your button is represented by a member variable name “myButton”.  In your code, you would listen to a button click by writing code that looks something like: You will notice that we used the `subscribe()` function instead of `map()`.  We still have a `map()` function.  But, `subscribe()` is how we tell the application, “we want to start listening to the stream now.”  Otherwise, `subscribe()` works just like map() does. Yes, I know what you’re thinking.  “How is that better than just having the template call an event handler?” Well, the fact of the matter is, it really isn’t all that much better.  But, here is where it does make more sense.

### Debouncing Keystrokes

If you’ve been writing application in JavaScript for a while, I’m sure you’ve written classic debounce handlers.  You know.  Don’t actually fire this event until you are no longer receiving change events from the input field. I won’t write out the old code here.  It is relatively long, hard to follow, and therefore somewhat complicated. But here is how we handle it using Observables. `debounceTime(250)` tells the Observable to wait for 250 milliseconds to see if there is some other event that comes in and use that event instead.  That is much easier than the old way.

### AJAX

While you could handle button clicks and debounce logic using old school JavaScript tricks, in Angular, it is practically impossible to make an AJAX call without using Observables.  This is because the `Http` service and the `HttpClient` that was introduced in Angular 4.3 use Observables instead of callbacks or promises to manage dealing with the data that eventually is returned from the AJAX call. Since `Http` and `HttpClient` are similar, we will continue our discussion of handling AJAX calls using `HttpClient`.  The main advantage to using `HttpClient` is that it handles parsing the response into a JavaScript object we can use.  `Http` just returns the raw Response object and parsing it out is up to us. `HttpClient`, on the other hand, returns the object we would have parsed out with `Http`. So, assuming you’ve injected HttpClient into the class that is going to use it, a typical get might looks something like this: So, walking through this, you may notice that some things look very similar to Promises and then, there are some other things that aren’t so much.  But trust me this gets much better.  We are just starting out small. First, what is that `TypeInfo` thing? You see, our get call is what is normally referred to as a “templated method.”  In simple terms, get doesn’t know what type it returns until you tell it.  So, we are telling it that it returns a `TypeInfo` type.  `TypeInfo` is just a name I made up.  You would create an interface that is relevant to the type of information that your AJAX code is returning. Other than that, we subscribe to the observable that get() returns and process the data. But what if our get call fails? Oh! We have methods for that. First, we can trap failures with a `catch()` call. But, maybe you want to `retry` the failed call before you give up. And because we have a `catch()`, with must have a `finally` too, right? Try doing all of that with a Promise or a Callback. Oh, and did I mention you can cancel AJAX calls using Observables?  Yep.  It’s true.  In fact, my experience has been that if you make the same call from the same service two times in a row, it will cancel the first call before it makes the second.  Pretty cool. The final thing that tends to trip people up who are learning about Observables is that nothing in the observable chain executes until you subscribe to the observable and an event happens. Once you start getting comfortable with all of the methods you have available to you, you’ll begin to see the power of using Observables over using Promises or Callbacks, even if there are similarities.
