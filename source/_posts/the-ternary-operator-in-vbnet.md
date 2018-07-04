---
title: The VB.NET Ternary Operator
tags:
  - tutorial
  - vb.net
  - visual studio
url: 41.html
id: 41
categories:
  - VB.NET
date: 2007-11-29 08:20:09
---

I think the VB.NET Ternary Operator may be the last operator that I really miss in VB.NET from my curly brace language experience.  Although, I have to admit, I wouldn't have missed it all that much if they never added it.  There just isn't a whole lot of use for it. However, the Ternary operator is a REALLY nice feature to have available to you when you do need it.  It's another one of those language features that falls under, "Just because it is there doesn't mean you have to use it." If you've ever run into a situation where you just need a simple evaluation and assign a variable based on it.  Like this:

        Dim s As String 
        If Session("mySessionVar") Is Nothing Then 
            s = String.Empty 
        Else 
            s = Session("mySessionVar").ToString() 
        End If 

  you'll appreciate the new Ternary operator which shrinks it to:

        Dim s As String 
        s = If(Session("mySessionVar") Is Nothing, _ 
               String.Empty, Session("mySessionVar").ToString)

Note that this NOT the same as

        Dim s As String 
        s = IIf(Session("mySessionVar") Is Nothing, _ 
               String.Empty, Session("mySessionVar").ToString)

  Here's the difference between the two. IIf will always evaluate the second and third parameter regardless of if the first parameter evaluates to true or false.  This is because IIf is a function, not an operator. If is an operator, and therefore only evaluates the second OR third parameter when they are the value that will ultimately be returned.  So, If() will run my code above without any errors while IIf will throw a null pointer exception when Session("mySessionVar") evaluates to nothing because it will try to apply ToString() to the object that is null