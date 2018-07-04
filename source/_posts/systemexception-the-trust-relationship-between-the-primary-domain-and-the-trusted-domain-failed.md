---
title: >-
  SystemException: The trust relationship between the primary domain and the
  trusted domain failed
url: 597.html
id: 597
categories:
  - ASP.NET
date: 2008-11-18 06:14:06
tags:
---

![A03B0015](/uploads/2008/11/a03b0015.jpg) I hate long titles, but this is what everyone is going to be searching for if they get this error, so that's the title today. I just got done fixing an error and when I went to Google, there were only five entries for this phrase and none of them solved the problem.  Some got me close and gave me a clue, but none of them said... 

> Hey dummy, you forgot to put in the forms authentication section in your web.config file and now you are asking for role information.  What gives?!

So, if you are getting this error, go check your web.config file for the forms authentication section.  I just did  a search in mine for "authentication" until I found the section that had been commented out. I'm not saying this IS your problem, but it was the solution to MY problem and it might just help someone to have it here.
