---
title: JavaScript Truthy and Falsy – Pain and Pleasure
tags:
  - falsy
  - javascript
  - truthy
url: 3318.html
id: 3318
categories:
  - Javascript
date: 2015-12-24 13:30:00
---

If you’ve been working with JavaScript at all for any length of time, you should know by now some of the basic rules of when something is true or false.  And yet, I still see code that messes this up. At the most simple level I see code that often looks like this:

var trueVariable = true;
if (trueVariable == true) {
    console.log('what a surprise, it is true!');
}

But the person who wrote this code, could have just as easily, and just as clearly written:

var trueVariable = true;
if (trueVariable) {
    console.log('what a surprise, it is true!');
}

Unless, of course, they wanted to know that the variable was both truthy and a boolean. In which case neither of the two code snippets above would be correct.  In that case, you would want to write the code like this:

var trueVariable = true;
if (trueVariable === true) {
    console.log('what a surprise, it is true!');
}

What do we mean by “truthy” or “falsy”? ![image](/uploads/2015/12/image-1.png "image") 

Truthy/Falsy Basics
-------------------

JavaScript treats any of the following values as “falsy”.

*   null
*   undefined
*   0
*   NaN (Not a Number.  The value, not the type)
*   an empty length string (‘’, or “”)
*   false

Everything else is truthy. Here are some code snippets that demonstrate the rules.

if (noneExistantVariable) {
    // this console.log should not run
    console.log("noneExistantVariable");
}

var aNullVariable = null;
if (aNullVariable) {
    // this console.log should not run
    console.log('aNullVariable');
}

var anEmptyString = '';
if (anEmptyString) {
    // this console.log should not run
    console.log('anEmptyString');
}

var aStringWithZeroInIt = '0';
if (aStringWithZeroInIt) {
    // this console.log should run
    console.log('aStringWithZeroInIt');
}

var aNumberWithZero = 0;
if (aNumberWithZero) {
    // this console.log should not run
    console.log('aNumberWithZero')
}

var aNumberOtherThanZero = 2;
if (aNumberOtherThanZero) {
    // this console.log should run
    console.log('aNumberOtherThanZero');
}

var aNoneNumericStringConvertedToANumber = Number('abc');
if (aNoneNumericStringConvertedToANumber) {
    // this console.log should not run
    console.log('aNoneNumericStringConvertedToANumber');
}

Mostly, writing our code like this adds a bit of convenience.  And yet, these shortcuts can also introduce unintended side effects.

Truthy/Falsy Dangers
--------------------

For example, I recently ran into some code that looks like this:

if (!obj.min) {
    obj.min = 'NA';
}

The intention of this code is that if min was null, or had not been defined on the object, we would set the property to ‘NA’.  But what if min is defined as 0?  In that case, we want to leave min as it is.  But this code will reset the value to ‘NA’ which is not at all what we wanted. There are several ways you might fix this code depending on what the surrounding code might look like. You might add a check to see if obj.min is numeric, but what if it accidentally got set to a string?  You might try converting obj.min to a number and then testing the type.  Remember, a string that isn’t numeric converts to NaN. In this case, I think the best solution is to test for exactly what we intended.  Is it not null and not undefined?

var obj = { min: 0 }
if (obj.min === null ||
    typeof obj.min === 'undefined') {
    obj.min = 'NA';
}

In fact, I use this code so much that I have created a utility class that has this as a function.

function isNullOrUndefined(v) {
    if (v === null ||
        typeof v === 'undefined') {
        return true;
    }
    return false;
}

What About !
------------

One final place you may get confused working with truthy and falsy is using the not operator, !. Just one quick simple example.  What if we take one of our first examples and place a ! in front of the variable we are testing?

if (!noneExistantVariable) {
    
}

Is this truthy or falsy? You may be inclined to think that this is still falsy because the noneExistantVariable is still undefined and putting a ! in front of it will not change the fact.  But, what you may not realize is that the ! converts the expression to a boolean and the expression !noneExistantVariable is not just truthy, but is in fact the boolean value true.
