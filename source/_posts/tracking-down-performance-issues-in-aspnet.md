---
title: Tracking Down Performance Issues in ASP.NET
tags:
  - asp.net
  - httpcontext
  - trace
url: 200.html
id: 200
categories:
  - ASP.NET
date: 2013-07-24 06:36:13
---

Last night, one of my clients assigned me a problem that I thought was going to require one solution, and in the end it was just poor programming. But the process reminded me of the need for good debugging skills. Just how do you know where the performance problem is? Too many programmers I know approach performance issues from the front end. "I know there is going to be a performance problem if I do it this way, so I'm going to do it that way instead." But unless your theory was correct, you are very likely to spend extra time doing something you may not need to do. While solving problems is what makes programming fun for a lot of us, solving problems that don't exist is a waste of time for the organizations we work for. My motto is:

<!-- more -->

*   Get the app working
*   Get the app working right
*   Get the app working fast

By following these steps, in this order, I very rarely even need to address step three. So, what happens when there is a performance problem? How do you track it down? Well, hopefully, you are addressing a visible performance problem. Something is taking more than a second to run. There are cases where you need to address performance problems that are less than a second and/or can't really be measured with a stop watch. But the biggest hits are the easiest to solve. So all you need to do is set some break points in your app and count how long it takes to get from one to the other. This is where a good layered architecture helps a lot. Very rarely is the performance issue in the display code. So you can set a break point in the code that gets the data out of the database. When I did this this morning, I found that there was a foreach loop in the BLL that was retrieving data that could just as easily be placed in the store procedure that we were calling right above the foreach loop. That seemed to solve the problem until we put the code on stage. And, this is where the fun began. While the code was definitely faster, it was still too slow. What was perplexing was that the data seemed to be the same in our test database as it was on staging. Worse, we no longer had access to our debugger. So, how do you track a problem like this down? This is where the tracing feature of .NET is of great value. If you've never used this feature, you can read some other articles about it elsewhere (I'll place links at the end) but for our purposes, we want to set up tracing a specific way.

1.  Place the following line in web.config

``` xml
    <trace enabled="true"
      requestLimit="10"
      pageOutput="false"
      traceMode="SortByTime"
      localOnly="false" />
```

This will enable tracing for your application but will not display the tracing information at the bottom of the page. Set localOnly to true if you can access the information from localhost--otherwise, you'll need to set it to false.

2.  Put tracing statements in your code that you are trying to get timing information for.

Now, here's the part many people don't know. While it is pretty simple to send trace statements out from the codebehind file using:

``` csharp
Trace.Write("message");
```

You can also access the Trace object and emit trace statements from your BLL and DAL code using:

``` csharp
HttpContext.Current.Trace
  .Write("message");
```

To access the trace output, run http://domaainstuff/applicationdirectory/trace.axd and select the round trip you want to examine.
