---
title: Decimal vs Float (Single) or Double
tags:
  - currency
  - decimal
  - double
  - float
  - floating point math
  - single
url: 495.html
id: 495
categories:
  - 'c#'
  - VB.NET
date: 2012-09-18 05:17:13
---

![money-016](/uploads/2008/10/money-016.jpg) When you need to deal with a number that is a fraction, what do you specify for its type?  If you are like most programmers I know, you'll reach for Float (Single if you are using VB) or Double.

If you are working with currency, though, this could get you into a lot of trouble.

Here's why.

<!-- more -->

When you store a number into a float, you are not storing an exact number.  This is because the number you are storing is an approximation of the number you entered.  When you store a number, the integer portion of the number gets priority and the fractional part gets entered as close as is possible given the size of the type you are storing it as.

That is, a Double will be able to save the information more accurately than the Float, but neither of them will store the information precisely.

And that's just storing a number we enter directly.  What happens if we need to multiply or divide that number?

For simplicity, let's just say we need to divide a dollar by three.  How would that get stored accurately into a float or a double?  The answer is that it won't.  It will store as many threes on the right side of the decimal as possible.

This is what the Decimal data type was created for.  A decimal data type deals with the data more like we would if we were dealing with the problem with paper and pencil.  In effect, a decimal data type is similar to an integer that has a decimal point.

Let me illustrate.  Let's imagine that we have an integer that holds our value and another that holds how many decimal places the value has.

``` csharp
int mainValue;
int decmialPlaces;
```

To represent a dollar we would say:

``` csharp
mainValue = 100;
decimalPlaces = 2;
```

Now, we want to divide our dollar by three:

``` csharp
mainValue = mainValue / 3;
```

obviously, mainValue will end up with 33 as a value, which means our result is .33, exactly what we would expect if we did this computation by hand.

If we multiplied the result by 3, we'd get .99, which is what we'd expect.  We still have to account for that lost penny, but we would deal with the lost penny using standard accounting practices.

Fortunately, we don't have to go to all that trouble because this is exactly why the decimal type was created for us.

``` csharp
decimal money = 1.00M;
money = money / 3;
```

I once went to consult in a project that was entirely currency based.  It was basically an accounting package.  This application was not only dealing with money, but it also had to deal with conversion rates between countries.  They wanted to know why they were losing pennies when they converted from one currency to another when the entire application was using floats (not even doubles) instead of decimal values.  That handled most of the problem for them.  The second recommendation I had was to choose a base currency type and always convert from that currency to all the other countries rather than converting from country A to country B and then back to country A.
