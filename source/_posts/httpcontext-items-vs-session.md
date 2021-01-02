---
title: 'HttpContext.Items[] vs Session[]'
tags:
  - httpcontext
  - items
  - session
url: 1464.html
id: 1464
categories:
  - ASP.NET
date: 2009-10-12 06:00:00
---

![ETHN0171](/uploads/2009/10/ETHN0171.png "ETHN0171")

Since .NET first became available, passing data around during a request has become a lot easier.  The ability to set a property has made that so.  Still, there are times when setting a property just won’t do the trick.

<!-- more -->

One such time is getting data from the middle tier back up to the view separate from a DataBinding operation.  That is, you databind a control to the middle tier and that method needs to set a value that will be used elsewhere in the view, not in the item that is being bound.

The natural, obvious tendency is to set a session variable.  But there is a better way.

The problem with session variables is that they have to be cleaned up manually or they will hang around longer than we actually need them.  This will use up more session memory than is required and can potentially cause side effects that will be difficult to debug.

Instead you can use the Items\[\] collection that is part of the HttpContext class.  It works the same as a session variable, but it only hangs around for the duration of the request.  Once the information is sent back to the browser, the variables that were set in the Items\[\] collection go away.

You might set your variable in the middle tier like this:

``` csharp
HttpContext.Current.Items\["myVar"\] = "Some Data Here";
```

And retrieve it later like this:

``` csharp
string myVar = (string)(HttpContext.Current.Items\["myVar"\]);
```
