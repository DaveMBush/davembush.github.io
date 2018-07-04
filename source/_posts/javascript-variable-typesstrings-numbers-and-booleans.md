---
title: 'JavaScript Alchemy with Strings, Numbers, and Booleans.'
tags:
  - javascript
  - type conversion
url: 3325.html
id: 3325
categories:
  - Javascript
date: 2016-01-01 13:30:00
---

Those who are relatively new to JavaScript might have the impression that JavaScript has no variable types.  After all, we declare everything using var and we can treat

if(1 == "1")

The same as

if(1 == 1)

or

if("1" == "1")

But the reality is that JavaScript includes a very rich typing system with some well-known, and some little know methods of detecting types and converting from one type to another. ![image](/uploads/2015/12/image-2.png "image") 

Strings and String Conversion
-----------------------------

The three most familiar types included in JavaScript are the String, Number, and Boolean types.  And one of the oldest tricks in JavaScript is the ability to convert a number to a string simply by concatenating a number on to a string.

var someNumber = 2;
var someString = 'abc';

var newString = someString + someNumber;
console.log(newString); // 'abc2'

In fact, anytime we “add” anything to a string, it turns it into a string

var someTruth = true;
var someString = 'abc';

var newString = someString + someTruth;
console.log(newString); // 'abctrue';

And it doesn’t matter where we put the string.

var someTruth = true;
var someString = 'abc';

var newString =  someTruth + someString;
console.log(newString); // 'trueabc';

Numbers and Number Conversion
-----------------------------

But what happens if the string contains a number and we add that to a number?  Well, it depends.

var someNumber = 9;
var someString = '3';

var newString =  someString + someNumber;
console.log(newString); // '39';

and

var someNumber = 9;
var someString = '3';
 
var newString =  someNumber + someString;
console.log(newString); // '93';

but

var someNumber = 9;
var someString = '3';

var newString =  someNumber + +someString;
console.log(newString); // 12;

Why? Well, prefixing the string variable with + converts that string to a number.  Under the hood, it is the same as if we had used the Number function to convert the string to a number.

var someNumber = 9;
var someString = '3';

var newString =  someNumber + Number(someString);
console.log(newString); // 12;

But what if someString can’t be converted to a number?

var someNumber = 9;
var someString = 'x';

var newString =  someNumber + Number(someString);
console.log(newString); // NaN;

and

var someNumber = 9;
var someString = 'x';

var newString =  someNumber + +someString;
console.log(newString); // NaN;

Boolean and Boolean Conversion
------------------------------

Last week, I discussed [Truthiness and Falsiness in JavaScript](/javascript-truthy-and-falsy-pain-and-pleasure/), but this week, I want to step back and discuss Booleans in the context of type information. You can convert any type to a Boolean value simply by putting a bang in front of it

if(!anyVariable)

It doesn’t just invert the truthiness or falsiness of anyVariable, it actually converts it to a Boolean first and then inverts the Boolean value. In some code, you may see

if(!!anyVariable)

because the person who wrote the code wants to test a Boolean value for true or false and not the truthiness of a non-Boolean value.  In my opinion, you don’t really gain anything by using this syntax.  But since you are likely to see it used by people who don’t really understand what JavaScript does under the hood without it, and think they are being more explicit and therefore making JavaScript faster (which they aren’t on both points) I include the syntax for completeness.
