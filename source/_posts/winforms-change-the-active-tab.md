---
title: WinForms - Change The Active Tab
tags:
  - active tab
  - selectedindex
  - selectedtab
  - tabcontrol
  - winforms
url: 962.html
id: 962
categories:
  - winforms
date: 2013-01-15 14:47:30
---

![misc_vol2_056](/uploads/2009/03/misc-vol2-056.jpg) This question came in last Friday:

> I'm trying to code a windows form in vb.net 2005. In my form I have 2 TabControls and a command button. The button is in the first TabControl, so what I want to do, is that when I click the button, in the first TabControl, the second TabControl gets opened.

I'm assuming here that what is really being asked is, "How do I change the active tab in a TabControl from some other event, like a button click in a tab?"

Given a form that looks like this:

![image](/uploads/2009/03/image2.png)

Create a Button Click event handler for "Button1" and set the SelectedIndex property of the TabControl to 1 to activate, "TabPage2"

    Private Sub Button1\_Click( \_
       ByVal sender As System.Object, _
        ByVal e As System.EventArgs) _
        Handles Button1.Click
        TabControl1.SelectedIndex = 1
    End Sub 

[](//11011.net/software/vspaste)If you don't know the index of the tab you want to activate, you can use the tab object instead:

    Private Sub Button1\_Click( \_
       ByVal sender As System.Object, _
        ByVal e As System.EventArgs) _
        Handles Button1.Click
        TabControl1.SelectedTab = TabPage2
    End Sub 

[](//11011.net/software/vspaste)
