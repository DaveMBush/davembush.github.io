---
title: ASP.NET Server Performance Testing
tags:
  - asp.net
  - memory
  - performance
  - testing
url: 2721.html
id: 2721
categories:
  - Testing
date: 2014-11-06 06:00:00
---

loading...

This happened a couple of years ago, but it is still relevant because I know of at least one place where it is still happening even though Microsoft has fixed the issue that initially caused this hack to be put in place in the first place.

Problem In Production – Oh NO!
------------------------------

Here’s the problem we were seeing.  We had several large PDF files that the client wanted to put up on the site so that their customers could download them.  The problem that we were seeing is that even though the site worked fine under development and QA, the site seemed to consume a lot of memory under load.

Another symptoms we saw was that the file download took a lot of time to start, or timed out completely.  But the main issue was the memory because when the memory was consumed, the site would restart, and because it was a worker process crash that caused it, the restart took the site down.  This is not something you want to see happen when you are working with a client with world wide exposure.  A client who, if I told you the name, I’m sure you would recognize.

In fact, it was the site not responding, or crashing, that first alerted us to the problem.  So, as soon as we knew the site wasn’t responding, I’d log onto the server and fire up task manager to see what was going on.

Server Management Tip
---------------------

One of the things I did early on managing the servers, is that I set up an entirely separate app pool for each of the web sites we are hosting.  This way, we can monitor the memory and CPU activity of each one independently.  For those of you who are interested, the way to make the pool name show up in task manager is to run the pool in ApplicationPoolIdentity, a setting you can get to through “Advanced Settings…”  If you are only hosting one site, you shouldn’t need to do this, but in my case, with multiple sites, this told me a lot about what was going on with the sites and ultimately helped me track down what the problem was.

By the way, you should always run each of your sites under a separate application pool so that when one site is having trouble, every site on your server doesn’t have trouble.  Imagine having an issue and not even knowing for sure which site was causing the trouble.  It’s bad enough not knowing what page is causing the problem.

It Is a Memory Issue.  Now What?
--------------------------------

OK.  So, I could see the memory increasing as soon as the site came back up.  So, now to try to track down what was causing memory issues.

I tried various things along the way, none of them shed any light at all on the problem.  I even got dotMemory from JetBrains to see if I could find anything.  A memory leak maybe?  No, none of my code had even a small leak.

The Fix Discovered
------------------

And then I finally stumbled on the issue.  I can’t remember what it was that we installed that needed this, but what it did was change our web.config file so that it had a section in it that looks like this:

<configuration>
... lots of other configuration stuff ...
  <system.webServer>
    <modules **runAllManagedModulesForAllRequests="true"**>

What that causes IIS to do is to run ALL of your web request through ALL of your .NET modules.  Why is this a problem you ask?  Because it even runs all of your images, css, javascript and other static content.  This isn’t normally a huge problem because most of that stuff is relatively small. But, when you try to run a be PDF through that pipeline, you’ll run out of memory because, and this is the major issue here, because the whole file has to be loaded into memory on the server before it can be sent on to the web browser to download.

Make sure that runAllManagedModulesForAllRequest is either set to false, or doesn’t exist in your web.config file.  If you need it to be turned on for some valid reason that I am not thinking of, at least put your static files on another site where this can be turned off.

Moral Of The Story
------------------

Now, the question that we should be asking now that we’ve figured it all out is, why wasn’t this caught earlier?  When a site goes up, isn’t all of the functionality, including all of the links to all of the images, files to download, and other pages on the site and off the site supposed to be tested?

Yes, of course they are, but if you haven’t at least written down where all of this is, if you don’t have a systematic way of testing EVERYTHING, you WILL end up with these kind of errors once you put the site live.  This is to say nothing of load testing along the way.  This can all be prevented. .csharpcode, .csharpcode pre { font-size: small; color: black; font-family: consolas, "Courier New", courier, monospace; background-color: #ffffff; /\*white-space: pre;\*/ } .csharpcode pre { margin: 0em; } .csharpcode .rem { color: #008000; } .csharpcode .kwrd { color: #0000ff; } .csharpcode .str { color: #006080; } .csharpcode .op { color: #0000c0; } .csharpcode .preproc { color: #cc6633; } .csharpcode .asp { background-color: #ffff00; } .csharpcode .html { color: #800000; } .csharpcode .attr { color: #ff0000; } .csharpcode .alt { background-color: #f4f4f4; width: 100%; margin: 0em; } .csharpcode .lnum { color: #606060; }