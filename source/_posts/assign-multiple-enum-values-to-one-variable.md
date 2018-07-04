---
title: Assign Multiple enum Values To One Variable
tags:
  - .net
  - enums
  - tutorial
url: 9.html
id: 9
categories:
  - 'c#'
  - VB.NET
date: 2007-11-02 07:56:52
---

I saw this question and immediately thought, "You can't!  An Enum is an Integer that has been restricted to the values it can accept."

And I was basically right.  But, I forgot that even with an integer you can do the following in CSharp:

![image](/uploads/2016/03/image-2.png "image")

int i = 1 | 2;

And in VB.NET

Dim i As Integer = 1 Or 2

To end up with a variable i equal to 3 because both do bitwise ands.

So if I had an enumerated value

enum F { 
    thing1 = 1, 
    thing2 = 2, 
    thing3 = 4
 }

Or, in VB.NET

Enum F 
    thing1 = 1 
    thing2 = 2 
    thing3 = 4
End Enum

You could then do the following in CSharp:

F fvar;

fvar = F.thing1 | F.thing2;

Or you could do it in VB.NET like this:

Dim fvar As F = F.thing1 Or F.thing2

There's just one small problem with doing all of this. 

If you evaluate fvar, you see that it is equal to 3 because we did not define 3 to be a specific value in our enumeration.  However, by adding the Flags attribute to our enum definition:

\[Flags\]enum F { 
    thing1 = 1, 
    thing2 = 2, 
    thing3 = 4 
}

Or

<Flags()> _Enum F 
    thing1 = 1 
    thing2 = 2 
    thing3 = 4
End Enum

fvar will evaluate to:

thing1 | thing2

in CSharp and in VB.NET...

Well, in VB.NET it still evaluates to 3.
