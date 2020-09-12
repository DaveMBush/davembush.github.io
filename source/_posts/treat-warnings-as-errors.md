---
title: Treat Warnings As Errors
tags:
  - compiler
  - errror
  - warning
url: 2499.html
id: 2499
categories:
  - Programming Best Practice
date: 2014-05-15 13:30:00
---

![TreatWarningsAsErrors](/uploads/2014/05/TreatWarningsAsErrors.png "TreatWarningsAsErrors")I used to ignore warnings when I compiled my code.  Most of the time they never caused any problems and I was able to run my code.  But recently I’ve gotten pickier about my code.  One area I’ve gotten more picky about is compiler warnings.

Any code that I have direct control over now compiles cleanly.  No errors and no warnings.  This past week I found a perfect example of why.  Given the following code:

<!-- more -->

``` csharp
public class Class1
{
  public virtual int PropertyOne { get; set; }
}

public class Class2 : Class1
{
  public int PropertyOne { get; set; }
}
```

You will get the following compiler warning:

> 'VirtualShadow.Class2.PropertyOne' hides inherited member 'VirtualShadow.Class1.PropertyOne'. To make the current member override that implementation, add the override keyword. Otherwise add the new keyword.

No big deal.  In fact, if you aren’t looking for it you won’t see it.

But here is the problem.

If you instantiate an object of Class2 and assign it to a variable of Class1 and then you assign the value 1 to PropertyOne using the variable of type Class1, if you tried to then access the same object from a variable of type Class2, PropertyOne will still be zero.  The default value for a property.

The default implementation is to treat Class2.PropertyOne as a shadow of Class1.PropertyOne, not as an override.  The warning message is telling you that it would prefer that you be explicit about what you really want the computer to do, which you have not done.

For this reason and others, I have all my projects set to treat warnings as errors in the compiler options.  It saves me from unexpected consequences like this one.
