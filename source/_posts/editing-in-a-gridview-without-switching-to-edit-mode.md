---
title: ASP.NET GridView Edit All Rows At Once
tags:
  - asp.net
  - gridview
  - tutorial
url: 5.html
id: 5
categories:
  - ASP.NET
date: 2007-10-30 07:23:15
---

I just saw a question about this yesterday and realized that while I know how to do this, not everyone does.  So, here we go...

Here's the problem.  You want to be able to edit all the rows in the gridview at once instead of having to switch to edit mode and save one row at the time.  Normally, you'd want to do this when only a couple of items need to be changed per row and not the entire row's worth of data.

![image](/uploads/2016/03/image-1.png "image")

Photo credit: [tico_24](//www.flickr.com/photos/tico24/16673795/) via [VisualHunt.com](//visualhunt.com) / [CC BY](//creativecommons.org/licenses/by/2.0/)

<!-- more -->

You can do this easily if you make the columns that need to be edited templated columns and place editable controls in them (checkbox, textbox, etc)  You can then either make these controls "AutoPostback" controls, or you can provide a control at the bottom of the screen that triggers the update.  In either case, the code you are going to write at the codebehind level is going to be the same.

For this example, we are going to assume that you only have one column that needs to be updated and that the control is a checkbox control.

One of the issues you are going to run into with this is that you'll need to know which row is associated with the control when it is updated.  The easiest way I've found of dealing with this problem is by adding a HiddenField control and databinding the row Id to it.  Since we are dealing with a CheckBox control, you will need to create an event handler for the Checked event.  The first parameter that will be passed into this event handler will be the sender.  Sender represents the control that fired the event.  In this case, it will represent the CheckBox control.

The other control you'll need to retrieve is the HiddenField control that you placed next to the check box.  You can retrieve this control by using the FindControl() method that is hanging off the parent control of the check box.  Assuming your HiddenField control is named "_hiddenFieldId" you can get the ID by using:

``` csharp
string id = (HiddenField)(((CheckBox)(Sender)).Parent
    .FindControl("_hiddenFieldId")).Text;
```

Now that you have the value of the ID and the value of the checkbox, you can update your database.
