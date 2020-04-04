---
title: Multi-Threaded JavaScript - Not The Problem You Think
tags:
  - javascript
  - threading
url: 3258.html
id: 3258
categories:
  - Javascript
date: 2015-10-22 07:30:00
---

A couple of weeks ago, I posted [7 Reasons Every Programmer Needs to Learn JavaScript](/7-reasons-every-programmer-needs-to-learn-javascript/).  In the comments, Dean tried to refute my arguments first by claiming that my sources for JavaScript’s popularity were “a problem” because JavaScript is used in combination with other languages.  A point I refute in the comments.  But then he goes on to claim that “JavaScript is poorly suited for client side applications” because JavaScript is “Single Threaded”.  At this point, I just sighed and realized that Dean doesn’t want to learn JavaScript and that there MUST be some reason I’m not seeing for why he is so critical of JavaScript.

But then Brandon jumped in and offered a very clear defense of JavaScript on the server side.  Nearly making this post unnecessary.

And yet, there are things that were not said, and most people will never see the great comments that Brandon supplied.  And so we look at Multi-Threaded JavaScript in depth.

![image](/uploads/2015/10/image2.png "image")

<!-- more -->

JavaScript vs Node.js
---------------------

In Brandon’s first comment, he accurately points out that it is not fair to equate JavaScript the language to Node.js the environment.  This is because Node.js does actually spin up multiple threads as needed to handle the incoming traffic.  In fact, there have been several performance test comparing Node.js to ASP.NET applications which have all found that Node.js either keeps up with, or out performs ASP.NET.  This is because of the way that traffic is handled.  In ASP.NET the default implementation is for an incoming request to block until the response for that request is sent back to the client, while the default mechanism in Node is for the request to NOT block.  In node, everything that should be none-blocking is none-blocking.  This is a HUGE win, essentially making node “Multi-threaded” in the places that matter without having to think about it.

> Speed comparisons:
>
> *   [Node.js Compared to ASP.NET without IIS](//www.haneycodes.net/to-node-js-or-not-to-node-js/)
> *   [WEBAPI vs Node.js](//mikaelkoskinen.net/post/asp-net-web-api-vs-node-js-benchmark)
> *   [Node.js Compared to J2EE](//blog.shinetech.com/2013/10/22/performance-comparison-between-node-js-and-java-ee/)
> *   [What makes Node.js Faster than Java?](//strongloop.com/strongblog/node-js-is-faster-than-java/)

Here’s a simple example.  You post data from the web browser to the server that needs to go into a database.  In ASP.NET, the default way of handling this would be to receive the request, send the data to the database, wait for the return value from the database, and return to the server.  All as a blocking call.

In Node.js, you would/could see this default behavior.  Information is sent to the server, the server sends to the database asynchronously and immediately returns to the client.  Of course, there are error handling issues that need to be addressed here, but the main point is that you don’t HAVE to wait and you probably shouldn’t.  If you need to send an error message back, that would be handled separately.

So, I ask, which is going to appear to be faster?

Who Needs Multi-threading?
--------------------------

OK.  So, we’ve addressed the environment issue.  But now I have to ask, when is the last time you really needed your application to be multi-threaded?  I can count the number of times that I used multi-threading capabilities in one of my applications (sever side or desktop application) one two hands in the last 28 years of programming.  The need for multi-threading is quite low relative to the number of programs written.  And even if you COULD write a multi-threaded application, the benefit compared to the complexity in your code tends to be relatively minor.

Multi-threading If You Need It – Client Side
--------------------------------------------

A little known and often under-utilized feature of JavaScript on the client side is Web Workers, which allow us to run a JavaScript file in a separate thread on the client side.  The “disadvantage” of using workers is that 1) they can’t access the DOM and 2) you have to use messaging to have the parent page and the worker talk to each other.  If you absolutely needed multi-threading on the Client Side, this would be the way to make it work today.  If you structure your code correctly, the separation from the DOM shouldn’t be an issue and messaging is a great way of communicating between a View Model and the View, even without the web worker process.

Multi-threading If You Need It – Server Side
--------------------------------------------

In the response I initially added to the multi-threading issue, I suggested running child processes.  This is the server side equivalent of using a worker process on the client side.  You spawn off some worker processes, wait for them all to return, collect the information they provide, and continue on.  As was pointed out, this is not “true multi-threading” because in a real multi-threaded application, every thread has access to the same memory space.  But you could argue that this is a good thing too.  There are a lot of issues with multiple threads accessing the same shared memory.  I’m not sure I would want the average programmer ABLE to do this.

Today is Not Tomorrow
---------------------

Just because JavaScript doesn’t handle REAL multi-threading today, doesn’t mean this won’t be added in the future.  The language is still evolving.  I could see a point in the next 5 years where this capability is added in, or a framework is created that will make this available, or both.

Only Use Multi-Threading Where You Need It
------------------------------------------

OK.  But what if you REALLY need Multi-threading capabilities? As was pointed out, then JavaScript may not be the right language for the job.  But then, you could easily argue that C#, Java, and many other languages aren’t either.  If you REALLY need that kind of speed, you probably want to drop down to C/C++ or even Assembly Language.  But why not mix and match.  It is rarely the case that your entire application needs to be multi-threaded.  In my experience, it is only some small percentage of the overall application that needs that ability.  So, why not create a node module that does the multi-threaded bit and wire it into your broader JavaScript application?

Wrapping It Up
--------------

Clearly, JavaScript is a young language and Node.js is a young environment.  There are still issues that need to be worked out.  But for as young as it is, they have some pretty robust capabilities “out-of-the-box” that will only get better in the future.

I’m excited and optimistic.  Excited about the potential and optimistic about the future capabilities for threading in JavaScript.

But maybe you aren’t.  You know what, while I think it might be a career mistake, it is your career.  You don’t have to agree with me.  And since neither of us are omniscient, only time will tell which of us made the better choice.
