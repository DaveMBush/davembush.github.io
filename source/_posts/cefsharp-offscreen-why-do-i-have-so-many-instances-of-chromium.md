---
title: 'CefSharp Offscreen [Why do I have so many instances of Chromium?]'
tags:
  - 'c#'
  - Chromium
  - debugging
url: 3241.html
id: 3241
categories:
  - 'c#'
date: 2015-10-01 07:32:00
---

I’ve been using the CefSharp.Offscreen library to drive the Chromium browser for a couple of months now.  While the code I’ve been working on has been working correctly, I could never figure out why so many instances of Chromium are left dangling in my task manager.  Oh, they’d all go away once I exited the application, but then it would take a very long time for my application to completely close because there were so many instances of Chromium hanging around. This past week, I finally figured out how to keep the number of Chromium instances in line with the number of off-screen browser windows I was actually creating. ![image](/uploads/2015/09/image4.png "image") I’m using version 41 of CefSharp, any future problem may not have this problem.  This post is intended to walk through the discovery steps and is not meant as a ding on the CefSharp developers.  Hey!  For all I know, the problem is in Chromium. So, as I’ve already mentioned, I noticed many instances of Chromium in my task manager.  At first I thought this was normal.  I’ve seen many instances of the Chrome browser in my task manager even though I only had one browser window open.  And I’ve seen information on the web that says multiple windows are needed to make Chromium work. But the more I run my program, the more windows show up in task manager.  Certainly this isn’t right. And then I started thinking about my code.  Every instance of my browser is wrapped in a using statement because the browser windows is disposable.  Could it be possible that some resource isn’t being disposed correctly as we use the same browser window to navigate from one page to another? Here is some code to illustrate my point.

// One browser window open at this point
// because of init code that runs before.
using(var browser = new BrowserObject())
{
    foreach(var item in listOfItems)
    {
       browser.LoadUrl(someNewLocation);
    }
}
// Multiple browser windows open here

So, obviously this isn’t right.  Well, at least it is obvious to me. But what if the use case for this never was intended for it to be used like a regular window.  In that case, putting the using statement inside of the foreach would solve my problem.  It isn’t quite as efficient as I would like, but at least it would work.  And the fact that I had so many instances of chromium running was eating up memory and slowing my whole computer down.  At least this would give me my computer back. So, I changed the code to look more like this:

// One browser window open at this point
// because of init code that runs before.
foreach(var item in listOfItems)
{
    using(var browser = new BrowserObject())
    {
       browser.LoadUrl(someNewLocation);
    }
}
// One browser window open here.

Fixed!
