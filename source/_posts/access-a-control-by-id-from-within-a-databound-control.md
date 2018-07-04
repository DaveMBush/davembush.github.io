---
title: Access a control by ID From Within a Databound Control
tags:
  - asp.net
  - controls
  - databind
  - databound
  - findcontrol
url: 1500.html
id: 1500
categories:
  - ASP.NET
date: 2009-11-02 02:21:07
---

![back-041](/uploads/2009/11/back041.jpg "back-041") Databound controls are at once very easy and very frustrating.  If you just need to do some simple databinding that gets a list of items on the screen and you need the ability to edit those items, you are all set. But once you need to do anything that deviates from that simple pattern, you are kind of stuck.  One of the problems involves accessing a control in a templated item.  Probably the most common place this shows up is in the FormView control.  But you’ll also experience the problem using the GridView control with a templated column. There is a pretty simple solution to the problem.  If you are in a form view, all you need to do is use the FindControl() method of any control.  The problem is finding which parent control houses the control we are looking for.  So what we really need is a recursive control that looks for the control with a particular ID in any of the controls that are a child of the control or any of its child controls or any of its child controls, etc., until it finds the control. I’ve written such a function for my utility box.

public static Control FindControlByID

(string id, ControlCollection controls) { Control returnControl = null; foreach (Control c in controls) { if (c.ID == id) return c; returnControl = FindControlByID(id, c.Controls); if (returnControl != null) return returnControl; } return returnControl; } Notice, it is a static method so we can call it from anywhere without having to instantiate the class it is in. So now, if I’m in a row and I need to find another control in that row, I can write code like this

Protected Sub m\_grid\_RowUpdating

(Object sender,GridViewUpdateEventArgs e) { int v= (DropDownList)(MyUtils.FindControlByID ("m_dropDownListId", (GridView)(sender) .Rows(e.RowIndex).Controls)).SelectedValue; } The only real tricky point left is that you may need to climb up to a parent to grab the right controls collection.  But once you figure how how far up you need to climb, the rest is academic. And before someone says it in the comments, obviously you’ll want to check to make sure you got the control using a if(var == null) statement prior to retrieving the value. **Other places talking about Finding Controls in ASP.NET** [gridview edit](//www.revenmerchantservices.com/post.aspx?id=b33bf4ef-3420-4013-8a45-4788285138bf) \- FindControl("txtFirstName"); TextBox tLastName = (TextBox)gr.FindControl("txtLastName"); TextBox tEmail = (TextBox)gr.FindControl("txtEmail"); Label lFirstName = (Label)gr.FindControl("lblFirstName"); Label lLastName = (Label)gr. ... [Using FindControl for a control in the ItemTemplate of a ListView ...](//www.joe-stevens.com/2009/09/18/using-findcontrol-for-a-control-in-the-itemtemplate-of-a-listview/) \- I've been playing around with the ListView control recently and am quite impressed with it. I like how it gives full control over the markup used as apposed.
