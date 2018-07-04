---
title: '&& vs & and | vs ||... What''s the difference?'
tags:
  - binary
  - bitwise
  - boolean
  - operators
url: 912.html
id: 912
categories:
  - 'c#'
date: 2012-08-21 04:12:38
---

![color-01](/uploads/2009/03/color-01.jpg) It seems like such a trivial thing to be talking about but not knowing the difference between && vs & or || vs | can make a huge difference between working code and code that only seems to work. Let me illustrate:

        bool b = false;
        bool c = true;
        if(b & c)
            // do something if(b && c)
            // do something 

In the code above, both b & c and b && c evaluate to false, so  we are safe.  No problems.  But this leads us to believe that the following code is also safe:

        string s = null;
        if(s != null & s.Length > 0)
            // do something if(s != null && s.Length > 0)
            // do something 

[](//11011.net/software/vspaste)and this is what would get you in trouble. The single ampersand and single pipe are knows as "bitwise operators."  What this means in practical terms is that they will take whatever they find on BOTH sides of the operator and AND (&) or OR(|) them together. So in the case of:

if(s != null & s.Length > 0)

[](//11011.net/software/vspaste)s.Length > 0 will still get evaluated even if s is null.  Not exactly what we had in mind. There is another side effect of bitwise operators that gets used very infrequently.  Because they are bitwise, I can use them on integers as well as boolean values.

int i= 5 & 6;

[](//11011.net/software/vspaste)is a perfectly legal construct in CSharp.  It will AND all the bits that make up 5  (101) and all the bits that make up 4 (100) and store the result in i (4 or 100). On the other hand, && and || are strictly used for boolean expressions and will evaluate as little of the expression as they can get away with. This is why we can write:

        if(s != null && s.Length > 0)
            // do something 

If s is null, we already know that the expression will fail, so there is no need to evaluate the string's length. And now for one of my favorite binary statements. There are 10 types of people in the world.  Those who understand binary and those who don't.
