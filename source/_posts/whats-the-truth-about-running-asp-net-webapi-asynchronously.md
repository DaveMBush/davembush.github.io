---
title: What’s the Truth about Running ASP.NET WEBAPI Asynchronously?
tags:
  - asp.net
  - async
  - await
  - webapi
url: 4090.html
id: 4090
categories:
  - ASP.NET
date: 2016-11-15 07:30:00
---

When Node.JS started getting popular, one of the major benefits people were proclaiming about it is that the web servers running under Node.JS were all processing the request asynchronously. This is how a single threaded environment was able to handle a significant load without falling over. Cool! So, you might wonder how does ASP.NET process request? It processes code synchronously. So, one might assume that if there were a way of running code asynchronously, we might be able to improve the performance of our applications. But can we? And if we can, is it worth it?

<figure>![](/uploads/2016/11/image-2.png "What’s the Truth about Running ASP.NET WEBAPI Asynchronously?") Photo via [VisualHunt](//visualhunt.com/)</figure>

<!-- more -->

Background
----------

The general idea of an asynchronous request is that a thread handles the incoming requests and immediately fires up another thread to handle the request. When the processing thread is done, it calls a callback function to notify the request thread that it is ready to send data back to the client. Meanwhile, the request thread has been able to handle additional request from additional clients. In contrast, a synchronous request processes the request itself so it is unable to handle any additional request.

In Node.JS, this is important because there is only one thread. It handles all of the request. If we had blocked, there would be no way of processing additional incoming request.

In ASP.NET, we are running in a system that can run multiple thread per process. So, we are able to handle multiple request because each thread is handled by a different thread. The problem, however, is that there are a limited number of threads that we can spawn per process and eventually we are handling so much traffic that we can’t handle any more. The longer our request take to respond, the more likely we are to experience this problem.

Imagine the kind of through put we could get on an ASP.NET application if we could make it run asynchronously, more like how Node.JS does.

So What’s the Problem?
----------------------

So, all of this sounds really good. But then when I went to try to find out how it works, I stumbled on a Stack Overflow question that basically indicated that it shouldn’t work. This, along with a project I was working on, lead me down the road of testing it for myself. Lesson: “Just because someone says something is true, doesn’t mean it is!”

The Test
--------

### Tools

**WestWind Web Surge (WWWS)** [http://websurge.west-wind.com/](//websurge.west-wind.com/) Simple load testing tool.

Add a URL, set number of threads to run concurrently, run, check report.

There is a command line version you can use as well.

**Strathweb.CacheOutput** There is a version for WEB API v1 and v2.

This allows us to add server and client side caching information to REST end points.

**Web Server** I’m using the web server built into Visual Studio 2015. I’m assuming IIS performs similarly.

All the tests were run on an 8 core computer with 16 gig of RAM. The numbers are for relative comparison. Your tests may show slightly different results.

### Baseline

For a baseline, I ran WWWS against this end point:

``` csharp
public IEnumerable<string\> Get() {
    System.Threading.Thread.Sleep(1000);
    return new string\[\] { "value1", "value2" };
}
```

Notice that while I am returning static information, I am simulating processing time with the Sleep() method. This makes it look like it took a minute to process the information prior to returning the information from the server.

I was able to run WWWS against the end-point for 20 seconds at a rate of 100 threads at a time without any errors. However, I was only able to achieve 28 request per second with an average response time of 4 seconds. Pretty slow once you see the optimizations.

### Asynchronous Optimization

The next test was to see what performance improvements I could get by running the end-point asynchronously.

``` csharp
public async Task<IEnumerable<string>\> Get(){
    await Task.Delay(1000);
    return new string[] { "value1", "value2" };
}
```

Note: you cannot just add the `async` keyword to the method, you must also have code you are `await`-ing inside your method. Otherwise the code will fall back to the baseline code. This is why I replaced Sleep() with Task.Delay(). It gives me something to await, while still waiting for a second prior to returning from the request.

Keeping all the parameters the same and running this code, request per second improved to 97. Average response time was just over 1 second.

I wasn’t able to get any failures within 20 seconds until I set simultaneous threads to 2000. At 1900 simultaneous threads the average response time was 9 seconds and the request processed per second was 444.

So while the response time was longer, more items were able to be processed by simply making the call asynchronous. So, that seems to prove that making the request asynchronous improves the performance.

Even Better Performance
-----------------------

But wait! There’s more.

What would it be like if we added caching onto this? By using the `CacheOutput` mechanism, we gain further benefit. While it might be difficult to add this in every situation, at least for static output this is something to be considered. I used the Strathweb implementation because it most closely implemented what had previously been available for WebForms and it now available in .NET Core The code

``` csharp
[CacheOutput(ClientTimeSpan = 50000,ServerTimeSpan = 50000)]
public async Task<IEnumerable<string>\> Get()
{
    await Task.Delay(1000);
    return new string\[\] { "value1", "value2" };
}
```

While WWWS doesn’t allow us to test client caching, it does allow us to test server caching.

In the code above I set the cache time to 50 seconds. Using the same 1900 threads as before, the average response time was 6 seconds with an average request processed per second of 347. I gave up maxing out the system at 3200 threads. This was returning results on average after 10 seconds of the request with request processed per second of 344.

If you were able to implement smart caching so new data was returned from the server only when something had actually changed, I’m sure you could achieve similar results with data that changed more frequently than your basic lookup tables.
