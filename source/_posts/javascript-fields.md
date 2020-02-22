---
title: JavaScript Object Fields
tags:
  - fields
  - javascript
  - methods
  - properties
url: 3835.html
id: 3835
categories:
  - Javascript
date: 2016-03-10 08:30:00
---

Last week as I was discussing the basics of [JavaScript Objects](/javascript-objectswhat-you-dont-know-can-hurt-you/), I kept referring to the members of the object as “fields.”  Never did I call them properties or methods.  This is because all members that are hanging off of an object are treated the same, from a membership perspective.  It is the type of data it contains that makes it behave as what we would normally refer to as a property or a method.

This is an important distinction.

In a strongly typed system, we can say that a member of our object is a property or method simply because it was defined as one or the other when we defined our class. In JavaScript we have neither classes where we can define what something is, nor strong typing.

So, how something functions is determined by the type of variable it is pointing to at run time. ![JavaScript Fields](/uploads/2016/03/image.png "image")  As I demonstrated last week, there are several ways that you might add a field to your object.

*   Use object literal notation and create them as you create the object.
*   Add them to the object after the fact using dot notation

But these are not the only ways. That was just enough so we could have the discussion about objects generally. As we have already seen, fields can be added to the object dynamically. There is nothing that restricts additional fields from being added. You can also delete a field using the `delete` key word.

Delete
------

In fact, one of the most common misconceptions with the JavaScript syntax is that the `delete` keyword is how you release memory. So, you’ll find code that looks something like this.

``` javascript
var a = 'abc';
// do some other stuff here
delete a;
```

And the people who write this code believe that ‘abc’ will somehow be removed from memory because they’ve done this. But that’s not how `delete` works. The proper way to use `delete` would look like this:

``` javascript
var a = new Object();
a.b = 'abc';
// do some stuff
delete a.b;
```

And what this would do is that it would remove the `b` field from the `a` object. The side effect would be that ‘abc’ would be released, but only if nothing else was pointing to it. It is just a side effect. `Delete` does not cause the memory to be released, it only enables that to happen if and when it is appropriate.

Key/Value
---------

If you are new to JavaScript, you might not realize that fields are just key/value pairs. End even if you do know this, you may not immediately realize all the implications this has. What this means is that you can either write your code to look like this:

``` javascript
var a = {};
a.b = 'abc';
console.log(a.b);
```

Or you can write it like this:

``` javascript
var a = {};
a['b'] = 'abc';
console.log(a['b']);
```

This means that we can create and access fields by using variables:

``` javascript
var a = {};
var fieldName = 'b';
a[fieldName] = 'abc';
```

Invalid Field Name
------------------

OK. Using a variable as a field name is pretty cool, but did you know that you can also name a field anything you want? That’s right. The only time it matters what you name a field is when you don’t use the key/value pair mechanism to create and access your fields. This means you can create a field that is any string that JavaScript will let you create.

HashMap
-------

Which leads to another powerful use of using keys for property names. Have you ever wished you could create a HashMap that used strings for the key to some value? Once again, it may look like the only type of list that JavaScript has available to it is the array. But by using this key/value pair mechanism for creating properties, we can actually leverage JavaScript’s properties as HashMaps.

For Fields in Object
--------------------

So, you might be thinking, if fields are essentially members of a HashMap, shouldn’t I be able to iterate through them? Well, actually, yes you can.

``` javascript
var a = {
    a: 'abc',
    b: function(){},
    c: 3
}

for(f in a) {
    console.log(f + ': ' \+ a\[f\]);
}
```

Will output

``` shell
a: abc
b: function(){}
c: 3
```

to the console. The variable `f` is the key and we use `a[f]` to get the value. I’ve used this feature to manipulate my code in a lot of creative ways including cloning objects. Be careful with this syntax. For/in is not the same as for/each and while it may appear to work like for/each, it was never intended to work on arrays. There are other ways of iterating through arrays.

Fields and Inheritance
----------------------

We will have a full discussion of inheritance in a future post. But for today, I just want to touch on the implications of how fields work in light of inheritance.

Let’s say we have an object `a` that inherits from object `b`. Object `b` has a field on it named, ‘firstName’ that has been set to ‘Dave’. Now, moving over to object `a`, we `console.log(a.firstName)` and what gets logged out is, of course, ‘Dave’. No big surprise there.

Next, we set `a.firstName` to ‘James’ and `console.log(a.firstName)` again. This time we get ‘James’ to display in the console. The question is, what is the value of `b.firstName`? You may be surprised to learn that `b.firstName` is still set to ‘Dave’ because when you set a field on an object, it is set on that object even if the parent object has the same field name. This is called "shadowing". Most of the time we don’t care about this because most of the time the end result is what we were expecting anyhow. But, there are times when, if you don’t know this is what will happen, you can shoot yourself in the foot.

Control Your Fields
-------------------

But what if you want a read only field, or you don’t want to have the field show up in a for/in listing? And what do we do about that shadowing issue I just mentioned? ES5 added a new feature that gives us a lot more control using the method named ‘defineProperty’.  It is unfortunately named 'define**Property**' because what makes it a property or not is how it is used, as I've explained.  But this actually works for both properties and methods. The basic syntax for defineProperty is:

``` javascript
var a = {};
a.defineProperty(propertyName, description);
```

Where `propertyName` is a string and `description` is a JavaScript literal in the form of:

``` javascript
{
    configurable: true/false,
    enumerable: true/false,
    value: someValue,
    writable: true/false,
    get: function() {return value;},
    set: function(value) { backingStore = value;}
}
```

Let’s step through these one by one.

### configurable:

The configurable field defaults to true. If for some reason you don’t want anyone to be able to change the definition of the field in the future, you would set configurable to false.

### enumerable:

Remember how we were able to use for/in to list out all of our fields. If you don’t want to have a field show up in for/in, you would set this configuration option to false. It is true by default. This option also controls if the property will allow you to list this property when you convert the object to a JSON string.

### value:

This will let you set the default value of the string.

### writable:

The default value for this option is false. You would set this to true if you don’t want the field to change for any reason. Two places where you might use this feature are:

*   If you create a string table for constants. These constants should never change, defining them as writable would be a good way to ensure this is true.
*   If you are creating a property that is an array. The common way of emptying an array is by assigning an array literal to it.`a.b = []`;but that assigns a new array to a.b instead of just resetting the length on the existing array object, which is probably what you wanted to do. This gets a lot of AngularJS programmers in trouble. By setting this field to false, you can still use `a.b.length = 0`; to reset the length, but you won’t accidentally assign a new array object.

### get:

The get option allows us to specify a function that returns the value of this field we are defining.

### set:

The set option lets us use a function to set the value. Aside from the obvious feature of being able to use a function to set the value, it has one other benefit. Remember that shadowing issue we talked about previously? If you have a field that uses a setter to set the field value, then the setter will get called instead of creating a shadow field in the child object.

So much to know
---------------

I bet you didn’t know there was so much to know about JavaScript fields.  It is amazing how much we can get done in programming when we only know a small fraction of what is available. If you found this helpful, don’t forget to sign up for the newsletter so you can learn even more about JavaScript.
