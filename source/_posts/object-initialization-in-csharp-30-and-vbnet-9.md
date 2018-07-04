---
title: Object Initialization in CSharp 3.0 and VB.NET 9
tags:
  - 'c#'
  - object initializers
  - tutorial
  - vb.net
  - video
  - visual studio
url: 40.html
id: 40
categories:
  - 'c#'
  - VB.NET
date: 2007-11-28 09:24:13
---

Yesterday we looked at the new var keyword in CSharp.  This makes CSharp variable declaration similar to VB.  After all, they've had the DIM keyword for years which essentially does the same thing. Today, we're going to look at object initializers, which have been added to both CSharp and VB.  Let's say we have a class named, "Customer" with the properties: FirstName, LastName, Address, City, and State.  If you wanted to initialize those properties as part of the object creation process you basically had two choices.  You could create a constructor with each of the properties represented as a parameter, or you could use the default constructor and then initialize each property individually immediately after you instantiate the object.  Using CSharp, that process would look something like:

        Customer c = new Customer() 
        c.FirstName = "Dave"; 
        c.LastName = "Bush";

  in VB you could write code similar to the CSharp code above, with obvious syntax changes for VB, or you could use the WITH keyword to simplify it.

        Dim c As New Customer() 
        With c 
            .FirstName = "Dave" 
            .LastName = "Bush" 
        End With 

  The new versions of these languages make the initialization process a bit easier.  Now, your CSharp code can look like:

        Customer c = new Customer() 
        { 
            FirstName = "Dave", 
            LastName = "Bush" 
        };

  and your VB code can look like this:

        Dim c As New Customer() With { _ 
        .FirstName = "Dave", _ 
        .LastName = "Bush" _ 
        }

  Keep in mind that the code that I just wrote compiles into the code I wrote using the old syntax.  This means that we can use this syntax in Visual Studio 2008 even if we are writing code for .NET 2.0.  Secondly, this means that it would still be faster to use the constructor with parameters if it is available. My fear is that some of the new features in the compilers will allow lazy programmers to write crappy code.  The point of the object initialization syntax is not to help you avoid creating parameterized constructors.  The point is to make your coding life easier when, and only when, the proper parameterized constructors do not exist.