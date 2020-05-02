---
title: JavaScript Scope
tags:
  - hoist
  - javascript
  - scope
url: 3124.html
id: 3124
categories:
  - Javascript
date: 2015-06-18 06:00:00
---

![ppl-men-060](/uploads/2015/06/ppl-men-060.jpg "ppl-men-060")

If you’ve been programming in any other environment than JavaScript for a while, you may be making assumptions about JavaScript Scope that just aren’t true.  One of those assumptions is how variables get evaluated when you run the JavaScript code and what variables are visible and at what point in the code they are visible.

Just as a test to see how well you know your JavaScript, let’s create a few tests scenarios.

<!-- more -->

Quiz
----

### 1) Given the following code:

``` javascript
var foo;
foo = "xyz";

function bar()
{
    foo = "abc";
    var foo;
}
bar();
console.log(foo);
```

What is displayed in the console window when console.log(foo) is executed?

Why?

### 2) Given the following code:

``` javascript
var i = 20;

for(var i = 0;i < 10;i++){
    console.log(i);
}

console.log('final i: ' \+ i);
```

What is the value of i when `console.log(‘final i: ‘ + i)` is executed?

Why?

### 3) What is wrong with the following code?

``` javascript
foo();

var foo = function(){
    console.log('foo was called');
}
```

### 4) Why would changing the previous code to this next block of code fix the issue with the previous code?

``` javascript
foo();

function foo(){
    console.log('foo was called');
}
```

Answers
-------

1) The value of foo when console.log(foo) is executed is “xyz”

2) The value of i when console.log(‘final i: ‘ + i) is executed is 10

3) foo is declared but undefined when we try to call it on the first line

4) because foo is both declared and defined when it is called on the first line

Why?
----

How did you do?

1) “Hoisting”
-------------

The first thing you need to understand about how JavaScript processes code is that it goes through the block of code you are working with and processes the variable declarations first.  That is anything in the global scope (window for browsers, global for server) or anything within a function block.  From the JavaScript compiler’s perspective, the code in example 1 looks like this:

``` javascript
var foo;
foo = "xyz";

function bar()
{
    var foo;
    foo = "abc";
}
bar();
console.log(foo);
```

So that the foo = “abc” line assigns the string “abc” to the variable foo in the bar function’s local scope.  Not impacting the variable foo in global scope so that the result at the end of the code sample is that the foo variable still has the value “xyz”.

2) Only functions and catch blocks create “block scope”
-------------------------------------------------------

In most languages that I use, if I wanted to create a variable that only had effect within a for, if, or while block, I could create a variable within the braces, or inline like in this example, and the code in the outer block would be left untouched.  But in JavaScript, this would only work if you were using the LET keyword which only appears in ECMA6 and above.

In JavaScript, it is perfectly legal to declare a variable multiple time.  The compiler will not complain.  So, when we run the code in example 2, the declaration within the for() is ignored and it just reuses the declaration at the beginning of the code snippet.

If you really wanted to declare a function within its own scope, you could create the scope with a try/catch block, like this:

``` javascript
try {throw i;}
catch(i){
    for(var i = 0;i< 10;i++) {
        console.log(i);
    }
}

console.log('final i: ' \+ i);
```

Which would give you the behavior you were probably expecting.  The code above will print out ‘final i: 20’ like you were probably expecting above.

3) “Hoisting” and function assignments
--------------------------------------

Going back to our discussion about variable declarations being processed first, and then doing any assignments, it becomes obvious that we can’t call a function that we haven’t assigned to the variable yet.  I don’t think I need to discuss this any further than I already have.  This works like any other variable declaration as I’ve discussed above.

4) “Hoisting” and function declarations
---------------------------------------

Function declarations, on the other hand, behave differently than function assignments.  In the case of function declarations, the variable AND the function that is “assigned” to it get pulled to the top of our code.  So using function declarations over function assignments is preferred simply because it assures us that the function pointer is available whenever we need it.

Conclusion
----------

So, maybe you thought you knew JavaScript.  Maybe you did.  Maybe you discovered you didn’t know it as well as you thought. I would encourage you to really learn the language.
