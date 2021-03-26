---
title: Why you can't cast an integer to a string.
tags:
  - .net
  - 'c#'
  - casting
  - convert
  - vb.net
url: 185.html
id: 185
categories:
  - 'c#'
date: 2008-06-23 05:59:21
---

I saw a question on Channel 9 that I've heard before. My guess is that there are enough people who have the same question that it's worth addressing.

> I know there's probably a really good reason for this, but I can't think of what it is, and it keeps bugging me. Why can't you do int x = 10; string y = (string)x; in C#? I mean, you could simply use x.ToString(), but why doesn't the explicit cast do the same?

<!-- more -->

## Why do you think you can do this in the first place?

The people who tend to ask this question are people coming out of a VB background. In VB, we have functions that will allow us to convert any type to any other type. CStr() is one of those functions. Give it an integer and it will convert it to a string. CInt() is another. Give it a value and it will convert it to an integer.

So, when people from that background first learn about casting, the only thing they have to hook the concept to in their thinking, is the conversion functions that are available in VB. You can't blame them, really.

But a cast is not a conversion. A conversion operation actually returns a completely different value. To do a similar operation in CSharp, you'd actually use the functions in the Convert class.

A cast, on the other hand, doesn't change the variable. It just lets us access it as though it were another type. And you can only cast if the data you are pointing to is related, via inheritance, to the type you want to cast it to.

## So the simple answer is, "because an int is not a string"

When dealing with an object-oriented language, particularly .NET languages, everything except for the Object type derives from some other object. So technically, a string IS AN object, an int IS AN object, in fact, everything IS AN object. If I have a class "Person" and a class named "Employee" that derives from "Person" and another class "Customer" that derives from "Person" I can say that an "Employee" is a "Person" and that a "Customer" is a "Person" but I can't say that a "Customer" is an "Employee".

Neither can I say that a "Person" is a "Customer", or that a "Person" is an "Employee". It doesn't make sense. So, if I have an object of type Employee, I can cast that object to a type Person. But I can't cast it to a type of Customer because there is no "is a" relationship between the two.

## So, how does this relate to Strings and ints?

Well, if you look at the inheritance tree for .NET, you will see that a String inherits directly from the Object class. You will also see that an int inherits from the ValueType class, which inherits from the Object class. Since neither type is derived from the other, you can't cast them--you can only convert them.

A little-known fact about VB.NET is that it also has a casting "function." It looks like a function to the VB guys, but it is really an operator disguised as a function. directcast() takes an object as the first parameter and the type you want to cast it to as the second parameter. The operator does the same thing that the casting operator in CSharp does.
