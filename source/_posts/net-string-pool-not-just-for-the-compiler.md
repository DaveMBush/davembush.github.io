---
title: .Net String Pool – Not Just For The Compiler
tags:
  - .net
  - 'c#'
  - intern
  - string
  - vb.net
url: 1034.html
id: 1034
categories:
  - 'c#'
  - VB.NET
date: 2009-04-22 04:34:00
---

![B03B0055](/uploads/2009/04/b03b0055.jpg "B03B0055") On Monday, I was corrected in my assertion that creating multiple empty strings would create multiple objects.  Turns out the compiler automatically puts all of the strings that are exactly the same in a “string pool” so that there is only ever one empty string in the entire application you’ve created.

<!-- more -->

Duh! I should have known this, or at least I should have expected that this was so since it has been true with every other compiled language I’ve worked with.

But what I didn’t know and couldn’t expect is that we can make use of this string pool programmatically as well.

**Why would you want to do this yourself?**

Keep in mind that string concatenation in .NET requires the creation of a new object.  So, code such as this,

``` csharp
String a = "abcd";
String b = "efgh";
a += b;
```

creates a new object at line 3 every time it is executed.

So that if we add the following line:

``` csharp
b = "abcdefgh";
```

we would not be pointing to the same object.  That is, a and b would contain the same content but would be pointing to two entirely different objects.

``` csharp
if (a == b)
    Trace.Write("A and B contain the same data");

if (String.ReferenceEquals(a,b))
    Trace.Write("A and B are the same object");
```

**String.Intern Consolidates The Data**

By using String.Intern() we can get both evaluations to be true.

``` csharp
String a = "abcd";
String b = "efgh";
a = String.Intern(a + b);
b = "abcdefgh";
```

Now both evaluations above will be true because line 3 places the string “abcdefgh” in the pool and line 4 uses that same string from the pool to assign to b.  Where we might have created two objects, we are now only creating one and referring to it both times.

You could also use String.IsInterned(string) to determine if a string has already been placed in the string pool and execute optional code based on that.

**When Would You Use This?**

I still stand by my statement that optimizations should be saved for last.  You would not do this if this was the only place where you were doing the concatenation.  But you might consider doing this if your concatenations were in a loop that was taking a significant amount of processing time.

Other things you might also want to consider would be to consolidate concatenations on the same line and/or using the StringBuilder class for concatenations.  Keep in mind that StringBuilder is only really useful once you get past three concatenations due to the overhead of creating the StringBuilder object vs. creating new objects during the normal concatenation process.
