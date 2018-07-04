---
title: VB.NET Hide Module Name
tags:
  - hide
  - modules
  - vb.net attributes
url: 1197.html
id: 1197
categories:
  - VB.NET
date: 2013-08-28 09:11:12
---

![misc_vol3_064](/uploads/2009/06/misc_vol3_064.jpg "misc_vol3_064") Here’s a quick tip for those of you still using modules in your VB.NET applications.

If you create a module and don’t want to see the module name in your intellisense, you can hide it with an attribute.  This can be extremely useful when you have a lot of modules that would show up in your intellisense code and they don’t have names that conflict with each other.

**_<HideModuleName()>_** _
Module Module1
    Sub NewFunction()
        Return
    End Sub

End Module 

[](//11011.net/software/vspaste)

You can still reference NewFunction() via Module1

Module1.NewFunction()

[](//11011.net/software/vspaste)But Module1 will no longer show in intellisense.
