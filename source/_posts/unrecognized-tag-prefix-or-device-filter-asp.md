---
title: Unrecognized Tag Prefix or Device Filter ‘asp’
tags:
  - asp.net
  - bug
  - filter
  - prefix
  - unrecognized
url: 1387.html
id: 1387
categories:
  - ASP.NET
date: 2013-07-31 06:58:06
---

![B01I0001](/uploads/2009/08/B01I0001.jpg "B01I0001") One of the companies I work for recently took over a project from another vendor.  As we started to maintain the site, we noticed that we could not drag and drop controls onto the page and get any more than a stub of  the control. <asp:hyperlink runat=”server”></hyperlink> is ALL we got when we dropped the hyperlink control onto the designer. In fact, every ASPX control on the page displayed the error, “Unrecognized tag prefix or device filter ‘asp.’” What’s up with that? Thinking that there must be something wrong with our development environment, or at least something wrong with how we have the project configured, I started my search on Google.  It seems the most common cause of this bug is that you have **nested master pages**.  The fix is to open the master pages in the IDE and leave them up while you work on the other pages in your application. Unfortunately, **that wasn’t our problem**.  We are only using one master page at a time. The other cause of the error that I found is defining the **master page in the web.config** file and not in the ASPX page. Again, keeping the master page  open fixes this problem. Again, **this wasn’t our problem**.  The master page is clearly defined in the ASPX page. Someone else suggested **deleting all the files in** c:\\documents and settings\\\[username\]\\application data\\microsoft\\visualstudio\\8.0\
eflectedSchemas Only **that directory doesn’t seem to exist** for VS 2008. Other solutions were in reference to beta versions of Visual Studio 2008, so that’s clearly not my issue. Up until this point I had been assuming that this code must have worked for the guys who originally developed the application.  But by this point, I was ready to throw that assumption out the window.  After all, this is the only application we have that exhibits this error. **So now it is time for the hacker's method of solving a problem**--move code around until you figure it out. One thing that looked a bit odd to me is that they were using a content area to control the body tag for each page.  They have a content area for controlling style sheet information too. Aside from the fact that they should be using themes to control the style sheet that is being used, or they should at least be using one style sheet with page-specific classes (I guess that’s a rant for another day), I thought the body tag stuff looked suspicious.  What happens if we move the body tag outside of the content place holder? **Bingo!** That one simple change fixed the entire issue. So the questions that remain are:

1.  How did this ever work for the original developers?
2.  Can we use DIV tags and classes instead of relying on the body tag?

We are supposed to have a meeting with the original developers today to get the answers.
