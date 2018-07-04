---
title: Multi-Step Processing in ASP.NET
tags:
  - asp.net
  - multiview
  - session
  - wizard
url: 1824.html
id: 1824
categories:
  - ASP.NET
date: 2013-05-08 01:32:51
---

![B01I0045](/uploads/2010/04/B01I0045.jpg "B01I0045")

I received the following question a few days ago but I’ve been so busy with billable work that I just haven’t had a chance to answer it until now.  Actually, I’m still busy, but I hate letting these questions go for too long.

“Right now I am working on a project where I have to screen a user.  This is a multi-step process.  At the end of the process I store the data back to the system. Currently, I am storing all the options a user will select in a session variable and then finally using them at the last step. Can you please suggest a better way to store this temporary data that does not require using a session? This type of situation comes up a lot.  We’ve used multiview to get it working. But this does not seem to be viable in all situations.”

If I had a multi-step process that I needed to complete, I’d probably use the ASP.NET Wizard Control, which is a lot like the MultiView control you mention.  The main difference is that it handles the navigation between the views for you.

If your process requires you to navigate between separate ASPX pages, then you’ll probably want to do something with cross-page posting.

Frankly, I don’t find session variables to be all that evil.  Your trade-offs are to either store all of the data on the page using hidden form variables or ASP.NET view state (using MultiView or Wizard controls), which makes the page heavier than it might otherwise be, or you need to store the information in session variables, which takes up memory on the server.

For most web sites the extra memory used on the server is not an issue because the site just doesn’t get that much traffic.

Unless we are talking about a 50-step process, storing the information in the page isn’t much of an issue either.

Since you never state what it is about the MultiView control that makes it not viable in all situations, I’m left puzzled.  Seems like its cousin, the Wizard control, is exactly what you need.  I’m guessing there is something you don’t understand about how these controls should be used.
