---
title: JavaScript Functions -- In-Depth
tags:
  - functions
  - javascript
url: 3393.html
id: 3393
categories:
  - Javascript
date: 2016-02-18 08:30:00
---

Last week I talked about [JavaScript variables gotchas](/javascript-variable-gotchas/).  This week, we want to take an in-depth look at JavaScript functions. Why? Well, for the same reason we looked at variables last week.  If you keep using JavaScript the way you think it works instead of the way it really works, at best, you will have a much harder time debugging your JavaScript code.  Worse case, you introduce some pretty nasty bugs into your code. So, let’s start with a pretty basic JavaScript function question.  One I would use as a question if I were interviewing someone for a hard core JavaScript job. What is the difference between the following two ways of declaring a function?

function foo(){
}

var foo = function(){
}

![JavaScript Functions -- In-depth](/uploads/2016/02/image-1.png "JavaScript Functions -- In-depth") 

Declaring JavaScript Functions
------------------------------

If you think these both do the same thing, you would be almost right.  But here is the key distinction. Remember our discussion last week about variable hoisting? When you declare a variable, the declaration is hoisted to the top of the variable scope but it isn’t assigned until the actual assignment in the code. So that the following code

var foo = function(){
}

Actually compiles to

var foo;
// other code may occur here
foo = function(){
}

On the other hand, if you declare functions using

function foo(){
}

What happens is that the variable is both declared and assigned at the top of the variable scope. In fact, when you use this second method to declare a function, the variable gets declared prior to variables declared with the `var` keyword.

Why Does This Matter?
---------------------

“So,” I hear you thinking, “why the fuss?  My code runs either way.” Or does it. If you use `var foo()` instead of `function foo()`, you can run into a situation where you have one function calling another function before the function variable has been assigned the function pointer.  It doesn’t happen often, and it happens a lot less frequently when you are doing “object oriented” JavaScript, but it can happen. Here is an example:

var foo = function(){
   bar();
}

foo();

var bar = function() {
}

Yeah, I know, wrong on so many levels.  But this is how bad code happens.  Implementing best practices at multiple levels makes the code more solid. If we rewrite the code to see what is happening the problem becomes clear.

var foo;
var bar;

foo = function(){
    bar();
}

foo();

bar = function(){
}

Bar is getting called before we ever assign the function to it. But by simply rewriting this using function declaration, we avoid the issue:

function foo(){
   bar();
}

foo();

function bar() {
}

When we rewrite this as it will compile, we see that we no longer have an issue:

function foo() {
    bar();
}
function bar(){
}
foo();

Anonymous Functions
-------------------

A discussion on JavaScript functions would not be complete if we didn’t address the subject of anonymous functions. Anonymous functions are functions that show up most often as parameters to other functions.  Rather than declaring a function and passing the function in as a variable pointing to the function

function callBackFoo(){
}

function mainFoo(callBack){
    callBack();
}

mainFoo(callBackFoo);

We get lazy and just write the function as part of the parameter.

function mainFoo(callBack){
    callBack();
}

mainFoo(function(){});

And here again, I hear you thinking, “Yep.  Do that all the time.  What’s wrong with that?”

The Problem with Anonymous Functions
------------------------------------

Well, there are two problems. First, when you use an anonymous function and an exception is thrown.  If your anonymous function is part of the call stack for the exception, all you will see in your debugger is something about “anonymous” because it doesn’t have a name associated with it.  Yes, you’ll get a file name and a line number.  But you’ll have to go look at the code to see what function caused the problem. The second, and I think more compelling, issue with anonymous functions is “Callback Hell.” You know, a function that takes a callback that calls a function that takes a callback … etc. If you haven’t seen this yet, you haven’t coded anything significant in JavaScript yet. This isn’t to say that I haven’t used these same shortcuts.  But they ARE issues you need to consider.  At least when I take the shortcut, I think to myself, “Is the pain really worth it?”

Immediately Invoked Function Expressions (IIFE)
-----------------------------------------------

Pronounced “iffy” the Immediately Invoked Function Expression is the one place where I think anonymous functions serve a very helpful purpose. The basic structure of an IIFE looks like this:

(function(){
    // code goes here.
})();

Or, if you prefer

(function(){
    // code goes here.
}());

The first way makes more sense to me.  But whatever. The idea is that we’ve created an anonymous function and executed it right away.  It is the same as if we had written:

function foo(){
    // code goes here
}
foo();

But, by doing this we’ve introduced a variable into our scope that we only plan to use once.  If we are doing this in our global namespace, we could be stomping over an existing variable we might want latter. Imagine if you were using jQuery and wrote:

function $(){
}
$();

or worse

var $ = function(){
}
$();

So, IIFEs take care of this issue.  It keeps all of the variables we declare in a file out of global scope while still making them accessible to all of the functions we declare within the IIFE.  Since it runs at load time, we really don’t need a function name for our debug stack.

A Note About JavaScript and Node.js
-----------------------------------

While everything I’ve said above is true for browser based JavaScript.  When you get into the area of Node.js development, the rules change slightly.  Specifically, each JavaScript file is already wrapped in an IIFE, this is why we need module.exports and why if you assign a variable in one JavaScript file it is not available to you to use in another JavaScript file.  So as far as I can think, there is no valid reason for using anonymous functions in Node other than laziness.  But I’m sure someone will set me straight in the comments ![Smile](/uploads/2016/02/wlEmoticon-smile.png).

JavaScript Functions Best Practices
-----------------------------------

All of this leads to the following best practices regarding the use of function in JavaScript

1.  Declare functions using the `function` keyword instead of the `var` keyword.
2.  Avoid anonymous functions
3.  Wrap globally accessible client side code in an IIFE to avoid polluting global variable scope.
