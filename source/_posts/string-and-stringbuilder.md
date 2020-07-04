---
title: String and StringBuilder
tags:
  - reference types
  - string
  - stringbuilder
  - value types
url: 2778.html
id: 2778
categories:
  - 'c#'
date: 2014-12-11 07:00:00
---

A couple of weeks ago, we discussed [Value types and Reference types](/value-type-vs-reference-type/) where we said that a reference type points to the value it represents and a value type is the value it represents. This has implications when we work with the assignment operator because when you assign a reference type and change the content of what it is pointing to, both variables get changed because they are both pointing to the same location in memory.  If you do this with a value type, only the one you change sees the change because you are working with a copy.

<!-- more -->

Is String A Value or Reference Type?
------------------------------------

So, if you’ve done any work with the String class, you might think it is a value type, because if you write this code:

``` csharp
string a = "abc";
string b = a;
string b = "def";
Console.WriteLine(a);
Console.WriteLine(b);
```

You will quickly discover that the values that get written out are: abc def Which is not what you’d expect if String is a reference type. So, the question we need to ask is, why is String acting like a value type if it is really a reference type?

Strings Are Immutable
---------------------

The answer is that Strings are immutable.  That is, a string never changes.  And if you are a thinking person I can already hear you saying, “Sure they change, just look at this code…”

``` csharp
string a = "abc";
a = "def";
Console.WriteLine(a);
```
"See, I changed the string variable a from ‘abc’ to ‘def’" And yes, you did change the a variable.  But what did you change?  You didn’t change “abc” to “def” you change what a was pointing to.

You see, “abc” is the object of type string and “def” is an object of type string.  All you managed to do was change what a was pointing to. In fact, if you write this code:

``` csharp
string a = "abc";
string b = "abc";
```

The result is exactly the same as if you’d written:

``` csharp
string a = "abc";
string a = b;
```

Because in .NET, there is only one instance of any given string in the system.  The duplicates get optimized out.

String Concatenation
--------------------

Now all of this has implications when it comes to concatenation. Let’s say you write this code:

``` csharp
string a = "abc";
string b = "def";
string c = a + b;
```

What’s happening here? First we create a string object that contains “abc” and point the a variable to it.  Then we create another string object that contains “def” and point the b variable to it.  And now this is where strings get interesting because the next thing that happens is that a NEW string object is created that contains “abcdef” and we point the c variable to that new value. Now, if you think about this for a minute, you’ll understand that this is incredibly inefficient.  Creating new objects is one of the most expensive operations that anyone can do is just about every object oriented language we have available.  In fact, I can’t think of one where this is not true.  I’m just assuming there must be an exception to the rule. It would be much more efficient if we were to make string so that it wasn’t immutable.  This would mean we could skip the create new object part of the assignment and our concatenation operations would work much faster.

StringBuilder
-------------

Enter StringBuilder.  StringBuilder is, essentially, the mutable version of the String class.  Now you can write code that looks like this:

``` csharp
string a = "abc";
string b = "efg";
string c = "hij";
var d = new StringBuilder(a);
d.Append(b);
d.Append(c);
string e = d.ToString();
```

Note, at the end, we still have to convert our StringBuilder object to a String object using ToString().  So, there is a new object creation penalty there.  What this means is that you probably don’t want to use a StringBuilder unless you are appending to a string more than three times. So, there you go.  That’s the difference between String and StringBuilder, why a String looks like a value type, and when you should use StringBuilder instead of String.
