---
title: 'C# “” better than string.Empty?'
tags:
  - 'c#'
  - empty
  - string
url: 1029.html
id: 1029
categories:
  - 'c#'
date: 2009-04-20 04:43:00
---

![arct-013](/uploads/2009/04/arct013.jpg "arct-013")I recently read an article that argued that “” is “Better than String.Empty”

The argument is that since string.Empty doesn’t work in all situations, we should not use it at all.  He further argues that since the compiler can’t optimize code using string.Empty, the performance gains we might lose due to our lack of this optimization further supports the argument that we should not use it at all.

But at what price?

<!-- more -->

First, it is impressive that he took the time to evaluate the performance hit that using String.Empty can cause.  I’m pretty sure his evaluation of using String.Empty in a case statement is from his attempt to do so only to find out he couldn’t.

However, he seems to have overlooked the price of not using String.Empty.  Certainly, Microsoft didn’t put that there without thinking about what they were doing.

So let’s further evaluate what is happening in our code when we use “” rather than using String.Empty.

**Consider Real World Optimization**

In the article referenced, he does one, and only one, bench mark to prove that “” is faster than String.Empty by putting the code in a loop that could be optimized out.

``` csharp
if (string.Empty == null) { throw new Exception(); }
```

vs

``` csharp
if ("" == null) { throw new Exception(); }
```

But what about a real world scenario where the code is NOT optimized out?

``` csharp
String s = String.Empty;
```

vs

``` csharp
String x = "";
```

In my test, there was no noticeable difference.  Sometimes string.Empty was faster and sometimes the empty string was faster.   And I expect the reason they are about the same is because the compiler optimized out the assignment.

In real life, I would expect String.Empty to take just slightly longer.  But not enough to make it worth worrying about.

**Consider String Comparison Cost**

Second, string comparisons are notoriously expensive in every language I’ve ever worked in.  Including the .NET languages.  Instead of arguing that we can’t using String.Empty in a case statement, we would do better to argue that using a string in a case statement is the last of the possible alternatives we might use.

When evaluating for the empty string, for example, you might check the string’s length rather than checking the string itself. For other strings, you might check the first character of the string.

**Writing Code is About Solving Problems**

When I started my career, computers were slow and had a limited amount of memory.  Writing the smallest amount of code that performed in the most efficient way was half the struggle of writing the application.

Today, neither of those issues are of primary concern.  The first order of concern is to write an application that works.  Once it is DOING what it is supposed to do, IF there are performance issues, we should do proper code evaluation to determine where the performance bottlenecks are and then, and only then, should we optimize our code for performance.

Generally, using String.Empty will serve you better than using “”.  In the cases where String.Empty will not work either because of the code evaluation OR because of the performance considerations, yes, use “” instead.  But, do it because there is a supportable reason, not because you think it may produce faster code.

In fact, I would argue that if your code has performance problems, the last place you should be looking is at this issue.  You’ll get negligible gains. Your real problem is more likely in file IO, including database access and network access.
