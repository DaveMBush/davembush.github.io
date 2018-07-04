---
title: JavaScript Boolean Confusion
tags:
  - boolean
  - javascript
url: 2601.html
id: 2601
categories:
  - Javascript
date: 2014-08-07 13:00:00
---

What could possibly be confusing about JavaScript Booleans you ask?

Well, here’s several logical statements written in JavaScript.  Do you know what each does?

if("0" == true)

if("0")

if("1" === true)

if(!!"0" == true)

if("0" != true)

if("0" !== true)

var someVariable;
if(someVariable)

if(someVariable == true)

if(!someVariable === true)

if(!!someVariable === true)

.csharpcode, .csharpcode pre { font-size: small; color: black; font-family: consolas, "Courier New", courier, monospace; background-color: #ffffff; /\*white-space: pre;\*/ } .csharpcode pre { margin: 0em; } .csharpcode .rem { color: #008000; } .csharpcode .kwrd { color: #0000ff; } .csharpcode .str { color: #006080; } .csharpcode .op { color: #0000c0; } .csharpcode .preproc { color: #cc6633; } .csharpcode .asp { background-color: #ffff00; } .csharpcode .html { color: #800000; } .csharpcode .attr { color: #ff0000; } .csharpcode .alt { background-color: #f4f4f4; width: 100%; margin: 0em; } .csharpcode .lnum { color: #606060; }

if(“0” == true)
---------------

"0" == true

.csharpcode, .csharpcode pre { font-size: small; color: black; font-family: consolas, "Courier New", courier, monospace; background-color: #ffffff; /\*white-space: pre;\*/ } .csharpcode pre { margin: 0em; } .csharpcode .rem { color: #008000; } .csharpcode .kwrd { color: #0000ff; } .csharpcode .str { color: #006080; } .csharpcode .op { color: #0000c0; } .csharpcode .preproc { color: #cc6633; } .csharpcode .asp { background-color: #ffff00; } .csharpcode .html { color: #800000; } .csharpcode .attr { color: #ff0000; } .csharpcode .alt { background-color: #f4f4f4; width: 100%; margin: 0em; } .csharpcode .lnum { color: #606060; }resolves to false.  Why?  Because both sides of the equal are converted to numerics and then evaluated.  This gives us, 0 == 1 which is in fact false.

However,

if(“0”) 
--------

is true.  This is because there is no conversion needed.  Any string is truthy.

OK.  So then, what would you expect

if(“1” === true)
----------------

to be?  If you said, “true” you would be wrong.  And just when you thought you were getting the hang of this.

This puzzle illustrates the difference between the double equals (==) and triple equals (===) evaluators in JavaScript.  You see, if you use double equals, JavaScript will always convert both sides to a common type and then do the evaluation.  However, triple equals says that both sides have to be the same type and the same value to be equal.  Since a String is not a Boolean type, “1” === true evaluates to false.

Do you think you are getting the hang of this yet?  OK.  What does the next statement evaluate to?

if(!!”0” == true)
-----------------

This evaluates to true.  If you thought it would be false, you probably applied the !! after the types were converted.  But the operator precedence on this is to apply the ! operators first.  So ! some valid string is false and ! false is true.  So true == true is true.

The ! operator
--------------

The next two examples I gave were to simply point out that != is the opposite of double equal and !== is the opposite of triple equal.  Beyond that, they work the same.

So, if(“0” != true) would evaluate to true and if(“0” !== true) would also evaluate to true.

if(someVariable)
----------------

The last set of examples illustrates what happens if a variable is undefined or null.

Since we never gave someVariable a value, it is undefined and so if(someVariable) would evaluate to false.  Undefined and null variables evaluate to falsy.

However, that does not mean that it is a boolean value.

So, if(someVariable == true) evaluates to false, if(someVariable === false) evaluates to false, if(!someVariable === true) evaluates to true and if(!!someVariable === true) evaluates to false.

Implications
------------

The main implication of all of this is that if you are going to compare two variables for equality, you should first convert them to a common type and then compare them using the triple equals operator.

However, if you are just evaluating a variable for truthiness, using just the variable, as in if(someVariable) is clear enough.  You don’t gain much by using if(!!someVariable) or even if(!!someVariable === true) syntax.