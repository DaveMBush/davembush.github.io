---
title: JavaScript Prototypal Inheritance
tags:
  - inheritance
  - javascript
  - prototype
url: 3908.html
id: 3908
categories:
  - Javascript
date: 2016-05-19 07:30:00
---

Over the last several months we’ve looked at several different aspects of how JavaScript deals with objects.  A few weeks ago, we looked at [JavaScript Types](/javascript-types-nuance/) and noted that many of the types are actually objects, while not all are.  We’ve also looked at [JavaScript Objects](/javascript-objectswhat-you-dont-know-can-hurt-you/) and [JavaScript Object Fields](/javascript-fields/).  This has all been foundational information you need to understand prior to understanding how JavaScript Prototypal Inheritance. ![JavaScript Prototypal Inheritance](/uploads/2016/05/image-1.png "JavaScript Prototypal Inheritance") 

No Classes
----------

If you are coming from an object oriented background, the first thing you need to understand is that JavaScript doesn’t have classes.  Even though the class keyword was introduced in ES2015, there are still no classes.  All the class keyword does for us is formalizes what we’ve been doing for years while making JavaScript feel more like the other languages we know. I’m not going to spend a lot of time dealing with ES2015 syntax here for several reasons.  First, it isn’t fully implemented in the browser eco system yet.  Second, most of what we do as programmers is maintain existing code.  There is a lot of existing code that doesn’t use ES2015 yet.  Third, ES2015 hides what is really going on.  I want you to understand how JavaScript works, not just be able to churn out code. So, if there are no classes, how does JavaScript achieve inheritance?  By using the delegation pattern.

Delegation
----------

In the object oriented world that you are probably coming from, you’ve probably heard the phrase, “Favor composition over inheritance.”  What they are really saying is, “Favor delegation over inheritance.”  So, this shouldn’t be a particularly new concept.  When you create a class that contains other classes, once the class is instantiated, when we need to call a function that the top level class doesn’t implement, we pass it on into an object that is contained by the top level object.  This is delegation. Now, remove the classes.  All you have left are the objects those classes would have created.  This is JavaScript.  But, instead of leaving the delegation to you, they’ve provided a default delegation mechanism called the prototype.  In fact, if you’ve ever inspected a JavaScript object in the debugger, you’ve probably seen this field hanging off your functions.  The other place you’ll see evidence of the prototype is in the \_\_proto\_\_ field that hangs off of every object.

Default Inheritance
-------------------

Whenever you create a new object using either an object literal, or a function (or the class keyword) the prototype field automatically points to the default empty object.  It is this default object that gives all of our other objects the behavior of an object.  Without this, none of our objects would have a default toString() implementation, for example.  It is the default object that gives all other object their object-ness.

Constructors
------------

Once your head stops spinning, come back and check this out.  While we no longer have classes, we still need some way of stamping out objects that all look the same.  We already looked at one way of doing this when we discussed [JavaScript Objects](/javascript-objectswhat-you-dont-know-can-hurt-you/).

function A(){
    var self = this;
    self.someProperty = 'A';
    self.someFunction = function(){
        self.someProperty = 'B';
    }
}

And for most of the code we write, this is a perfectly adequate way of creating a constructor. By attaching the functions to the function’s prototype field, we can apply the functionality one more level up the tree, which gives us a certain amount of flexibility. The same code above could be written as:

function A(){
    this.someProperty = 'A';
}

A.prototype.someFunction = function(){
    this.someProperty = 'B';
}

Notice that we didn’t attach someProperty to the prototype.  We want the state information attached to our object.  If you did attach it to the prototype, all it would do is give the object a default value of ‘A’ but as soon as we assign ‘B’ to it, the property gets shadowed anyhow.  If you were to Object.define() someProperty so that it had a setter, which would remove the shadowing, you would also change the value for every instance of the object A when you changed it from any instance.  I suppose if you wanted to implement something that looked like a static variable, this is something you might attempt. The key to remember here is that anything you do to the prototype is going to impact all current and future instances of the object.

JavaScript Prototypal Inheritance
---------------------------------

By now, I hope you understand that all inheritance happens by delegation through the prototype.  The next obvious question would be, “How do I make one JavaScript ‘class’ inherit/delegate to another ‘class’?” One way you might be tempted to implement inheritance is by assigning prototypes.

function A(){
}

A.prototype.foo = function(){
}

function B(){
}

B.prototype.bar = function(){
}

B.prototype = A.prototype;

But all this does is make B inherit from the same thing A inherited from.  Not exactly what we wanted to see happen. OK, you say.  I know what to do, I’ll just create a new object of type A and assign THAT to the prototype of B.

B.prototype = new A();

You’re closer and it may work a lot of the time, but if your A function that you are using to create that other object does anything, you may end up not doing what you expected.  For really simple objects, this will work, but it is a dangerous habit to get into. What you really want to do is to use the Object.create() function.  This creates a new object without calling the constructor function.  No side effects.

B.prototype = Object.create(A.prototype);

But, what if that A constructor function did something important? In your B constructor function, you call the A constructor function passing it the current this pointer.

function B() {
    A.call(this);
}

If B takes parameter than need to be passed on up to A, you can pass those additional parameters after this in your call to call(). And that is how we make JavaScript inherit one object from another.  It is a lot of work.  This is why ES2015 introduces the class and extend keywords.  They do a lot of this work for us.
