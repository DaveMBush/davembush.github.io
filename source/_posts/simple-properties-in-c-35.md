---
title: 'Simple Properties in C# 3.5'
tags:
  - 'c#'
  - tutorial
  - visual studio
url: 35.html
id: 35
categories:
  - 'c#'
date: 2007-11-22 05:49:00
---

It's such a little thing.  But, how much of our CSharp code looks something like this:

``` csharp
private string _propertyName;

public string PropertyName
{
    get { return _propertyName; }
    set { _propertyName = value; }
}
```

<!-- more -->

When I teach other programmers how to use CSharp (or VB) I always stress the importance of using properties instead of public member variables.  You never know when you'll want your set to do some sort of validation and just about all of the databinding stuff requires us to use properties instead of member variables.  But, that's a lot of code to write when all you want to do is wrap a member variable. Well, in CSharp 3.5, life just got a lot sweeter.  That code above just got replace with this:

``` csharp
public string PropertyName { get; set; }
```

You can still use the code above if you want to.  But, why write all that code, even if you write it using a code snippet, when you can just write that one line?
