---
title: Accessing Private Fields in TypeScript
tags:
  - private
  - testing
  - typescript
url: 4218.html
id: 4218
categories:
  - TypeScript
date: 2017-02-28 07:30:00
---

Have you ever needed to access a private field in TypeScript?  The most common place you may find yourself needing to do this is while writing a unit test.  But, I also found myself needing to do this while using a JavaScript library where the field wasn’t declared in the type file for the library I was using. Now, suppose you could access those private fields effortlessly and easily.  How valuable would that be to you? <figure>![](/uploads/2017/02/image-3.png "Accessing Private Fields in TypeScript") Photo via [VisualHunt](//visualhunt.com/)</figure>

<!-- more --> 

Unit Test
---------

In general, when you write a unit test, you only want to access public fields for both the purposes of setting up your tests and for evaluating the success or failure of the tests. But, occasionally, this is not possible. Now, what most people don’t realize is that, unlike private members in JavaScript, where the members aren’t accessible, in TypeScript, the resulting JavaScript has the variables just as public as the public members.  In fact, the only thing that makes a member private in TypeScript is the compiler. This means that this TypeScript code:

class Foo {
    private member1: string;
    private bar() { }
}

Ends up looking something like this in JavaScript

function Foo() {
    this.member1 = null;
    this.bar = function() { }
 }

Which means that

var v = new Foo();
var x = v.member1;
v.bar();

Should be working code. But, if you type that code into JavaScript and try to compile it, it won’t compile.  Which means you can’t write your unit test in TypeScript and access the private variables. Or can you?

TypeScript is just JavaScript with Sugar
----------------------------------------

One small little fact about TypeScript that we seem to forget is that it is just JavaScript with some sugar.  What this means in practical terms is that, if we want to, or in this case, need to, we can write plain old boring JavaScript as part of our TypeScript code. And then the other little bit we tend to forget is that we can access a field using the name of the field as an indexer. That is, this:

v.member = 'x';

is functionally the same as this:

v\['member'\] = 'x';

And because it all compiles down to JavaScript, and the private fields are public JavaScript fields, we can use the named index to access the field.

JavaScript Libraries
--------------------

Similarly, this past week I was working on finishing up some Angular 2 code.  And one of my tests was failing.  Even though the code was working in Chrome fine.  The issue was that I was using PhantomJS which doesn’t have the latest JavaScript spec implemented, so it relies on polyfills.  One of the polyfills I was using was not compressing the Regular Expressions that I was indirectly using correctly which resulting in the test throwing an exception. After tracking down the source of the problem for a day, I finally found a line at the bottom of the GitHub page that told me that I could turn the routine off by calling a function.  And here is where the trouble begins. You see, I’m using an otherwise documented internal library that has a set of types already defined for it.  This particular function is not a part of the types for this class.  So, when I tried to call it, I got a compiler error. So, I pulled out the named index trick above and got the code to compile and ultimately got my test to run successfully. It all just requires that we think outside the box a bit and most any problem can be solved.
