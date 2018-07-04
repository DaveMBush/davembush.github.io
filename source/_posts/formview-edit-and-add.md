---
title: FormView Edit and Add
tags:
  - asp.net
  - 'c#'
  - formview
url: 154.html
id: 154
categories:
  - ASP.NET
date: 2008-05-12 08:11:50
---

The FormView generally makes editing a record at a time pretty easy.  However, one of the biggest problems I've seen with this control is that there is no way of telling the FormView to use the same template for both Add and Update.  My experience has been that most of the time, Add and Update look exactly the same, or enough the same that you could use one template and hide the inappropriate controls in the template. One of my first attempts at fixing this problem was to use a WebUserControl.  However, I quickly discovered that you can't databind a WebUserControl.  That was in Visual Studio 2005.  I'm not sure if it's changed in 2008, but I assume it hasn't. I finally discovered a great way of using the Edit template for both the Add and Edit operations.  Simply by making use of the three tiered archietecture, you can overcome this limitation of the FormView control.