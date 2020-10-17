---
title: VB.NET Nullable Types
tags:
  - nothing
  - nullable
  - value types
  - vb.net
url: 1307.html
id: 1307
categories:
  - VB.NET
date: 2014-01-15 21:46:57
---

![tp_vol4_001](/uploads/2009/07/tp_vol4_001.jpg "tp_vol4_001") SQL has long had the ability to specify that a value is NULL even if it is a primitive type, but the only way you could have a NULL value in VB.NET is if you were dealing with an object. That is, until .NET 2.0

<!-- more -->

Even though .NET 2.0 has been out for a while, I would bet that few VB.NET programmers know about this new feature because it is one of those things most of us have grown to assume is not possible. Values must have content--objects don’t.  That’s just the way it is.  If we were to create an integer for example and then tried to assign a nothing to it, we would most certainly get a compiler error.

``` vb
Dim i As Integer i = Nothing
```

However, the Nullable generic was added so that we can create a VB.NET Nullable Integer

``` vb
Dim i As Nullable(Of Integer)
i = Nothing
```

And by simply putting a question mark at the end of our variable name, we can shorten the syntax to

``` vb
Dim i? As Integer i = Nothing
```
