---
title: CSharp adds the var keyword!
tags:
  - 'c#'
  - tutorial
  - visual studio
url: 39.html
id: 39
categories:
  - 'c#'
date: 2007-11-27 07:56:11
---

There have been several new features added to the CSharp language that will significantly reduce the amount of code that ends up in our source files.  It will not significantly reduce the amount of code that we have to write. One of those language features is the ability to create properties, [which we looked at last week.](/2007/11/22/simple-properties-in-c-35/ "Simple properties in CSharp") Another of those features is the new var keyword. So, instead of writing:

 MyClass c = new MyClass();

  you can now write:

 var c = new MyClass();

  Which isn't a lot of code until you start qualifying the Class name with namespaces. During the beta cycle, I saw a demo that let you declare a variable, var c, and then several lines later initialize it with, new MyClass(), which gave the appearance that var was more like the var keyword in javascript, and therefore a variant than what it really is. In the release version of CSharp 3.0, if you use the var keyword to declare a variable, you MUST initialize it on the same line, or you will get a compiler error.  I suppose it makes writing the compiler a whole lot easier this way too. One other small thing to note, which should be obvious by now.  Since we have to initialize the variable to some object or value, and since we can't initialize it anywhere other than on the line it is declared on, you can't treat the variable as a variant.  A variable declared as var is as strongly typed as any other variable you would create.  So, if I did something like this:

    var c = new MyClass(); 
    c = "Some string here";

I would get a compiler error because I'm trying to assign a string type to a MyClass variable.  var does not stand for "variant," it stands for "variable."  All the compiler does when it sees this is look at the type being assigned to the variable and replaces the var keyword with that type. So, when the compiler processes the code, it takes this:         var c = new MyClass(); and turns it into this: MyClass c = new MyClass(); Finally, it may be helpful to point out here that this whole process happens at compile time.  This should be obvious by the fact that this works in both .NET 2.0 code compiled with the CSharp 3.0 compiler as well as .NET 3.x code.  But, sometimes the obvious isn't obvious until someone states it explicitly.
