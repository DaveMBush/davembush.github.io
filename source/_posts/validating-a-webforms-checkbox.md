---
title: Validating A WebForms Checkbox . . .
tags:
  - asp.net
  - validation
  - visual studio
url: 67.html
id: 67
categories:
  - ASP.NET
date: 2008-01-04 09:05:24
---

**. . . and other ASP.NET controls that the Validation controls can not be wired to.** The presentation today may be something you already know how to do.  But, this question comes up repeatedly in my work as a .NET coach, which means there are still people who don't know how to do this.  There are other people who think they know how to do this but are hacking the solution.  I encourage you to watch the video.  My bet is that 80% of you that do will learn something you didn't already know about the validation controls and how to use them properly. Here's the basic problem.  There are controls in the .NET framework that can not be wired to the standard validation controls.  The checkbox control is one example.  You can't use the RequiredFieldValidator because it has a value.  It's either true or false.  And you can't provide a RegularExpressionValidator or one of the others because it is a boolean value. So, if I want to make sure a check box is checked before the user continues, for example.  And, I want to make sure that the error message shows up in the error summary control like every other error, how do I do that?