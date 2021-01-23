---
title: Manually Adding Event Handlers in VB.NET
tags:
  - events
  - vb.net
url: 1286.html
id: 1286
categories:
  - VB.NET
date: 2009-07-15 06:43:09
---

![office-019](/uploads/2009/07/office019.jpg "office-019")

Typically when we write our code, the event handlers get wired up for us using the handles clause.  So we never have to worry about wiring up our event handlers manually.

But what about the case where we want to dynamically add a control to our Windows Form or our ASP.NET page?  For example, add a button.  How would you respond to the button click event?

In CSharp, there is no handles clause, so figuring out how to manually wire up the event handler is simply a matter of inspecting the dotNet code and doing a copy/paste/modify operation in the editor.

The syntax for adding event handlers manually is not that difficult.

``` vp
AddHandler m_button.Click, AddressOf buttonClickMethod
```

If you’ve written any threading code, you’ll notice that this looks similar to the code you might have written for that.

The AddHandler statement takes two parameters.  The first is the event we are going to handle--in this case, the click event from the object that m_button is pointing to.

The second parameter is a pointer to a function that will handle the event.  What is unique about this is that it can be a method that is part of the current class, which is what the code above is referencing, or it can be a method in another object, or even a method that is shared in another class.

To reference a method in another object

``` vb
AddHandler m_button.Click, _
    AddressOf SomeOtherObject.buttonClickMethod
```

To reference a shared method

``` vb
AddHandler m_button.Click, _
    AddressOf SomeClass.buttonClickMethod
```

Which gives us quite a bit of flexibility when we dynamically wire up our events.
