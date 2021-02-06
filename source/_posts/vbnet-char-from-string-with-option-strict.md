---
title: VB.NET - Char from String with Option Strict
tags:
  - char
  - split
  - strict
  - string
  - vb.net
url: 982.html
id: 982
categories:
  - VB.NET
date: 2009-04-08 04:35:14
---

![G04B0079](/uploads/2009/04/g04b0079.jpg) So here's the question:

> I'm using String.Split() and need to pass in a Char or a Char array as the parameter.  If I pass in a string String.Split("/") I get an error "Option Strict On disallows implicit conversions from 'String' to 'Char'."
>
> Obviously, the easiest way to fix this would be to turn off Option Strict, but I would prefer to keep it on.  So how do I pass in the Char instead of a String in this situation?"

There are actually several ways to accomplish what you are trying to do.

The first and most general solution would be to call the ToCharArray() method off the string.

``` vb
Dim strSplit() As String = myString.Split("/".ToCharArray())
```

The advantage to this method is that it will work regardless of what size the string is and it will use each character in the string as a delimiter.

But what if you only have one character in your array?  Surely there is a shorter, cleaner statement we can use.

As a matter of fact, there are several other options.  You could use Convert.ToChar() or Char.Parse()

``` vb
Dim strSplit() As String = _
   myString.Split(Convert.ToChar("/"))
```

or

``` vb
Dim strSplit() As String = _
   myString.Split(Char.Parse("/"))
```

But the easiest way to convert a single character string to a Char is simply to put a "c" after the closing quote:

``` vb
Dim strSplit() As String = myString.Split("/"c)
```
