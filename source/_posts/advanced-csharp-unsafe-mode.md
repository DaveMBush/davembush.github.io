---
title: 'Unsafe Mode in C#'
tags:
  - 'c#'
  - pointers
  - unsafe
url: 658.html
id: 658
categories:
  - 'c#'
date: 2008-12-15 08:14:08
---

![tp_vol4_006](/uploads/2008/12/tp-vol4-006.jpg) One of the "advantages" of using CSharp instead of VB.NET is that if programmers want to, they have the option of bypassing the memory management of .NET and working with memory directly.  This is called "unsafe" mode. While I will show you how to use this keyword, I have to tell you up front that I've been using CSharp since Beta 2 of .NET 1.0 and I've NEVER needed to switch into unsafe mode to do any of the work that I've done. I've even written code that bridged down to some unmanaged C++ code and still have not needed to use unsafe mode.  One of the main advantages of using .NET to begin with is the fact that .NET manages our memory for us.  I've worked on far too many C++ programs that leaked memory due to their complexity and bad architecture to purposely go back to that model. Yes, it is true that you might get a slight performance improvement by bypassing the memory management and working with the memory directly.  But is that slight improvement worth the risk of all of the issues that arise when using memory directly? If you find that you need to use unsafe mode, I would recommend that you consider writing that part of your code in a language that was designed for that level of coding.  Assembler or C++ are some good choices. If these are not options, here's where the unsafe keyword comes in. Any code you wrap in the unsafe keyword:

unsafe {
    // this code is unmanaged }

becomes unmanaged.  You will also need to add the /unsafe compiler switch to your compiler options. You can also make an entire method unsafe by adding the keyword to the method declaration:

public unsafe void Foo(int *i)
{
    // i is a pointer that is unsafe }

In a future post, we'll look at some ways of using pointers in CSharp code.
