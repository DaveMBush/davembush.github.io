---
title: CSharp Numeric Overflows
tags:
  - 'c#'
  - checked
  - double
  - float
  - int
  - long
  - short
  - unchecked
url: 2562.html
id: 2562
categories:
  - 'c#'
date: 2014-07-03 13:00:00
---

![NumericOverflow](/uploads/2014/06/NumericOverflow.png "NumericOverflow")Did you know that when you are dealing with numbers, by default, .NET will do, or try to do exactly what you tell it to do?  If you tell it to do the impossible, it will do the next most logical thing.  It won’t throw any errors in the process.

No, neither did I.  But then, most of the time I don’t write code where this would matter, and I bet you don’t either.

So here are some examples.

Integer Types
-------------

What happens if you write code that looks like this?

    long l = long.MaxValue;
    int i;
    i = (int) l;

Well, if we are working with the compiler set with default settings, you’ll end up with the variable l having the value of 9223372036854775807 and the value of i having the value of -1.

Why is this?  Because, by default, .NET does not check for numeric overflow.

However, if you wanted to add the ability to check for numeric overflow, you would wrap this code in a checked block.

    try
    {
        checked
        {
            long l = long.MaxValue;
            int i;
            i = (int)l;
        }
    }
    catch (Exception ex)
    {
        // handle overflow exception here 
    }

You can force all of your code to be checked by changing your compiler settings.  In Visual Studio 2013, go to Project Properties –> Build.  Click the “Advanced” button (bottom right corner) and then check the “Check for … overflow” checkbox.

Once you’ve done that, all of your code will be checked by default.  If you don’t want to check for overflows while you are working on a block of code, for performance reasons, wrap the code in an unchecked block instead.

Float to Int
------------

Now, what about putting a floating point number in an integer?

Let’s say that instead of a long, we use a float.

First, let’s look at a simple case:

    float l = 3.563f;
    int i;
    i = (int) lf

In this case, you’ll end up with f holding a floating point number of 3.563 and i holding an integer of 3.  The rule is pretty simple, floats always have the decimal portion stripped off and assigned to the integer, short, or long.

Large Float to Int
------------------

But what happens if we use float.MaxValue?

    float f = float.MaxValue;
    int i;
    i = (int) f;

In this case, what you’ll end up with is a very large floating point number and an integer of -2147483648.  Basically it does it’s best to convert a really large integer number into the int and just uses what it can.  Not really what you probably have in mind.

The solution is the same.  Use a checked block or compile with checked turned on.

Double to Float
---------------

Now, you would expect that if I had a really large double and tried to assign that to a float, something similar would happen. 

You’d be right, kind of.

If you assign a double that is too large for a float to a float variable, the float variable will end up with infinity.  But, unlike the shorts, ints, and longs, you can’t wrap the code in a checked block to cause it to throw an exception.  No.  In this case you must always check for infinity once you’ve done the assignment.

    double d = double.MaxValue;
    float f;
    f = (float)d;
    if(float.IsInfinity(f))
        // do something intelligent here

  
So there you have it.  This is how you really deal with numeric casting in your CSharp code.
