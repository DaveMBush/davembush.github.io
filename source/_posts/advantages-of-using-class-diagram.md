---
title: Advantages of Using Class Diagram
tags:
  - 'c#'
  - class diagram
  - vb.net
  - visual studio
url: 606.html
id: 606
categories:
  - Did you know
date: 2008-11-20 08:02:47
---

![misc_vol4_063](/uploads/2008/11/misc-vol4-063.jpg) One of the new tools that showed up in Visual Studio 2005 that I don't see many people taking much advantage of is the Class Diagram.

<!-- more -->

The class diagram displays the classes you drag onto it in a visual representation much like a UML class diagram does.  It also lets you see relationships between your classes.  But the greatest advantage of the Class Diagram is that it will write a lot of your code for you.  The Class Diagram is available in both CSharp and VB.NET and works similarly in both.  My description of the tool will be using CSharp in Visual Studio 2008.  There may be a few quirky differences if you are using VB.NET and/or Visual Studio 2005. I was reminded of this tool a couple of days ago when I needed to override a method but I couldn't remember its name.  I could have spend a few minutes looking in the parent class for the name of the method I needed to override, but instead I created a new Class Diagram file and did a drag and drop of the class I was working on onto the Class Diagram's surface.  This then let me right-click on the class and select "Intellisense" > "Override members..." from the context menu.

This will bring up a dialog that will list ALL of the classes the class inherits from (so it helps to know what class the method you want to override is in).  You can then check off the members you want to override from the list supplied.  When you press OK, the methods will be stubbed out for you in the source code.  All you need to do is provide the functionality. You can use this same type of process to add new methods, add properties, and add member variables.

If you haven't broken out the class diagram recently, I suggest you give it a try.
