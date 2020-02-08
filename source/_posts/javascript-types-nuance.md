---
title: JavaScript Types Nuance
tags:
  - javascript
  - types
url: 3889.html
id: 3889
categories:
  - Javascript
date: 2016-04-28 07:30:00
---

I was once teaching a class on JavaScript to a group of C# developers when someone asked a very logical question, “Are JavaScript Types all derived from Object?” I loved teaching this particular group because they were actively engaged in the material.  So many times, when I teach, the students simply absorb what I say, but they don’t interact with it.  They never ask the question, “What are the implications of what is being said.” My initial instinct was to say ‘no’ based on my experience with the language.  But then as I thought about it later, I thought, “But when I use the debugger on what seems to be a primitive, don’t I see it as an object?”  And as it turns out, my instinct was right.  Not everything in JavaScript is an object.  Although there is quite a bit that you wouldn’t think was an object that is.

Now that we’ve covered [JavaScript Objects](/javascript-objectswhat-you-dont-know-can-hurt-you/) and [JavaScript Object Fields](/javascript-fields/), it is time to move on to the specifics of JavaScript types.

So, why is it, when I look at some primitive values, I see them as objects?  And which types are objects and which are primitives?

<figure>![](/uploads/2016/04/image-4.png "image") Photo via [Visualhunt](//visualhunt.com/)</figure>

<!-- more -->

A Review of JavaScript Types
----------------------------

The fundamental types available to us in JavaScript are:

*   undefined
*   boolean
*   number
*   string
*   object
*   null

However, if you use the `typeof` operator on null, you’ll get back “object” as the type.

While `null` is a unique type, it makes sense for `typeof` to return “object” since the only kind of variable that could return a `null` would be an `object`.

When is an object not and object?
---------------------------------

There is one other common type that is a bit of an odd ball.  The function type.

What makes function odd is that it is, technically it is a sub-type of object.  This is good to know, and will put you light years ahead of your peers once you realize the implications.  Because a function IS an object, you can give a function additional fields.  In fact, a common way to override a function looks like this:

``` javascript
a = 'abc';

var originalSubstring;
var substringOverload = function(a,b){
    return originalSubstring(a,b);
}

originalSubstring = a.substr;
a.substr = substringOverload;

console.log(a.substr(0,1));
```

(Note: the code above won’t really work, I’m just illustrating a point).

You may have done something like the above using functions in libraries.  As long as the field is not read-only, you can do this kind of overload of a function.

But, a better way, now that we know that a function is just an object, is to assign the old function as a field of the original function:

``` javascript
a = 'abc';

var substringOverload = function(a,b){
    return substringOverload.substr(a,b);
}

substringOverload.substr = a.substr;
a.substr = substringOverload;

console.log(a.substr(0,1));
```

What about Arrays?
------------------

Another place you may not be used to thinking clearly about variable types is with Arrays.  You might think an array is its own type.  That an Array is an Array.  But in reality, Arrays are a type of Object.  In fact, if you were to run the typeof operator against a variable that holds an Array, you would see that it is an object.

Once again, because you know this, you can use this information to your advantage.

You could provide your array, its own implementation of each:

``` javascript
Array.prototype.each = function(callback){
    for(var i = 0;i < this.length;i++){
        callback(this\[i\]);
    }
}

var a = \[1,2,3\];

a.each(function(item){
    console.log(item);
});
```

This is essentially how polyfills are created.  If you write one, make sure you put in the additional code to make sure the function isn’t already implemented.  And don’t ever add a function to a native object like this without it having been declared by the standards committee as a function that is part of the spec.  Polyfills exist so that you can make older JavaScript implementations work as though they were using newer standards.  Not so we can add our own new functions to the language.  If you do, you could find yourself having a maintenance nightmare on your hands some day in the future.

Newing a Type
-------------

You can also write JavaScript that looks like this:

``` javascript
var someNumber = new Number(3);
var someString = new String('abc');
var someBool = new Boolean(true);
```

Which will give you an Object that contains the value we passed in.  And each of those object will have Number, String, or Boolean functions available to it.

But, you don’t have to new a Number, String or Boolean to get those functions.  You can get the same ability by simply assigning the value to the variable.  Under the hood, when you want to use the function that are available to all objects, the JavaScript runtime will “box” the number, string, or boolean as an object so that you can access, for example, hasOwnProperty().
