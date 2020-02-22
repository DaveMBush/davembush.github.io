---
title: JavaScript Objects -- What You Don't Know CAN Hurt You
tags:
  - javascript
  - object
url: 3802.html
id: 3802
categories:
  - Javascript
date: 2016-03-03 08:30:00
---

I’m assuming that anyone reading this blog has probably been using JavaScript for a while.  Many of you have used a number of the many frameworks that are available and most have used jQuery.  For the most part, you get what needs to be done, done.

But, I would also say that most of you have no idea how JavaScript works.  This is why I’ve written about [JavaScript Variables](/javascript-variable-gotchas/), [JavaScript Functions](/javascript-functions-in-depth/), and now [JavaScript – Objects](/javascript-objectswhat-you-dont-know-can-hurt-you/).

So, let’s start with the most basic of JavaScript questions.

![image](/uploads/2016/02/image-4.png "image")

How Do You Create A JavaScript Object?
--------------------------------------

Well, I think this is a basic question, but you would be amazed at how few people can answer this, even though they’ve probably done this an innumerable number of times.

This simple statement:

``` javascript
var a = {}
```

creates an object and assigns it to the variable `a`.

Of course, it isn’t very useful at this point and it is functionally the same as if we’d written:

``` javascript
var a = new Object();
```

Notice that I said, “functionally the same.”  What is going on under the hood isn’t exactly the same, but the result is the same.  In both cases, you end up with an essentially empty object that is assigned to the variable `a`.  So, what is the difference between these two methods?

In the first case, we are assigning an object literal.  `{}` is the object.

In the second case, it is the `new` keyword that creates the object.  This object is then passed into the `Object` function and is accessible inside of the Object function as the `this` keyword.  It is the same as if we had written our own function that looked like this:

``` javascript
function Object(){
}
```

Now that we have an object, we can attach fields to it that we can use as properties and methods of our object.  And, once again, we can achieve this result in multiple ways.

Make Your Object Useful
-----------------------

The easiest way to add fields to our object is to do it as part of creating our object literal.

``` javascript
var a = {
    someProperty: 'A',
    someFunction: function(){
    }
}
```

Which we can later access like this:

``` javascript
var b = a.someProperty;

// and

a.someFunction();
```

But, if you needed to create multiple objects that all look the same, this could get rather tedious.  Fortunately for us, JavaScript provides us the ability to initialize objects using JavaScript functions.  So, the equivalent code using a function initializer, would look something like this:

``` javascript
function A(){
    this.someProperty = 'A';
    this.someFunction = function(){
    }
}
```

And now when we create a new object using the `new` keyword

``` javascript
var a = new A();
```

We can call the fields in the same way we did when we used the object literal.

A Word about `this`
-------------------

You’ll notice that we are using `this` to represent the object that was passed into the function.  But using `this` in JavaScript is not the same as using `this` in other languages such as C#, Java.  In those languages, `this` is “the object that I am.”  In JavaScript, `this` is “the context I was called from.”

What?!

OK.

Let’s back up.  Remember, in JavaScript we are using a function to initialize the object.  How did that object get passed to the function?  It got passed to the function as the function’s context.  JavaScript has no concept of an object that is assigned to a specific function.  There are no classes in JavaScript that keep associations between the resulting object and the code it is associated with.  In JavaScript, everything is an object.  And, if you want to, you can call a function passing it whatever object you want!

In fact, this happens naturally all the time.

If you use a JavaScript function as an event handler, the object that is passed to that event handler is the DOM element that fired the event.  Probably not what you would have in mind.

If you call a JavaScript function directly from the global variable context, the context that you would be calling from is the global object.  To illustrate, if we called our `A()` function without using `new`, we would create a global variable `someProperty` and a global function `someFunction` as a result of that call.  Once again, probably not what you would normally expect.

However, if you call a JavaScript function directly from the global context and you have `"use strict"` defined, `this` is now `undefined`.

Making `this` Behave
--------------------

At this point you should be thinking, “Ug! If I can’t rely on `this` to be any specific object, how does object oriented JavaScript even work?”

Well, if you remember our discussion about variables and closures, you should be able to derive the answer.  The way most of us deal with this specific issue is that we assign `this` to some other variable when we initialize our object using the initialization function, and then we use that variable instead of `this` throughout the rest of our code.

Let’s say that we want someFunction to access someProperty:

``` javascript
function A(){
    this.someProperty = 'A';
    this.someFunction = function(){
        this.someProperty = 'B';
    }
}
```

In order to ensure we were always accessing the right context, we assign this to a variable and use that variable.

``` javascript
function A(){
    var self = this;
    self.someProperty = 'A';
    self.someFunction = function(){
        self.someProperty = 'B';
    }
}
```

There are a few standard naming conventions for the variable that represents the original context.  You can use self, like I’ve done above.  I’ve also seen that used. _this is one that I’ve seen but I find way too confusing to be useful. Finally, I’ve seen a variable with the same name as the initialization function.  Assuming the initialization function is upper cased, the variable would be the same name only lower cased.

Finally, if you want to be a purist and use the most recently sanctioned way of handling the `this` problem, you can use `bind()`, depending on how far back you need to support browsers.

If you did want to use `bind()`, then you could setup your code like:

``` javascript
function A(){
    this.someProperty = 'A';
    this.someFunction = function(){
        this.someProperty = 'B';
    }
    this.someFunction.bind(this);
}
```

Which would ensure that `this`, really is the `this` you are expecting.

As we move toward ES2015, using `bind()` will become more important. But I believe today you are more likely to see the first syntax, as long as we are still using functions to create our classes. Once we move to ES2015 syntax, using `bind()` will become more common.

Similar but Different Objects
-----------------------------

We now have a very convenient way of creating new objects that do something useful.  But, what if we wanted someProperty to start with something other than ‘A’.  What would we do?

One awkward way to handle this would be to create the object and then change the value of the property.

``` javascript
var a = new A();
a.someProperty = 'B';
```

But we are using a function to initialize our object.  Why not just pass it a parameter with the value we want to use?

``` javascript
function A(_someProperty){
    var self = this;
    self.someProperty = _someProperty;
    self.someFunction(){
        self.someProperty = 'B';
    }
}
```

End
---

There is a lot more to JavaScript objects than I’ve covered here.  If you are interested in going deeper, make sure you [subscribe to the newsletter](/news-letter/) so that you don’t miss the next post.
