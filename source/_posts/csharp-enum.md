---
title: CSharp Enum
url: 556.html
id: 556
categories:
  - 'c#'
date: 2013-10-02 12:57:23
tags:
---

![tp_vol4_002](/uploads/2008/11/tp-vol4-002.jpg) It is often tempting when working with your code to test against static values in your system.  For example:

if (i == 1)
{
    // do something }
else if (i == 2)
{
    // do something }

Or, if you are a bit more clever, you use a switch statement instead.  But neither of these methods tell you what 1 and 2 actually represent. Now, you could use constants:

const int Thing1 = 1;
const int Thing2 = 2;

var i = 1;
if (i == Thing1)
{
    // do something }
else if (i == Thing2)
{
    // do something }

Which goes a bit further.  But if I need to use Thing1 and Thing2 in other sections of my code, I'm kind of stuck.  Oh, I guess one could use a class with properties and evaluate against them, but there is a better way. Enums. When you have a set of items that are all related to each other, you can create an enumeration (enum) to represent them as a set.  There are several of these built into the .NET Framework.  One of the most common is the DialogResult enum that holds all of the return values for a DialogBox. We can create our own by either placing code similar to the following in an existing file or in a new code file:

enum Thing {
    One,
    Two
}

Which can then be used in our code as:

switch (i)
{
    case Thing.One:
        // do something break;
    case Thing.Two:
        // do something break;
}

In the example above One represents zero and Two represents the value of one.  But it really doesn't matter because we are always going to reference the value by name. You are not stuck with having to have Thing.One be one.  You can assign a value to it or any of the other values.

enum Thing {
    One =2,
    Two
}

In the code above, One equals the value 2 and Two equals 3.  Each item in the list is automatically incremented by one unless it is overwritten with a value. In the case of items that need to be bit representations you can do something like:

enum Thing {
    BitOne = 1,
    BitTwo = 2,
    BitThree = 4
}

And then your code can evaluate them using bit wise and statements:

if ((i & Thing.BitTwo) > 0)
{
}

  This will be true if i is equal to 2, 3, 6, or 7.  The expression above is a lot easier to evaluate than "if i is equal to 2, 3, 6 or 7." Aside from the practical ease of coding that enums provide, it also makes the code much clearer.  Which would you rather do?  Lookup what the return value for OK is or just have intellisense provide it to you from the DialogResult enum?  Obviously, having it provided by its name is much easier.  You should provide the same benefit to any future developer who works on YOUR code.
