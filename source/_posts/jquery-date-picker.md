---
title: jQuery – Date Picker
tags:
  - asp.net
  - datepicker
  - jQuery
url: 1226.html
id: 1226
categories:
  - jQuery
date: 2009-07-02 07:28:00
---

![tp_vol4_002](/uploads/2009/07/tp_vol4_002.jpg "tp_vol4_002")

Today we’ll start taking a look at the final control in the jQuery UI suite.  The Date Picker.

This control, as with all the other controls we’ve looked at, is relatively simple as long as you keep in mind that you need to use the jQuery UI Styles with it.

So let’s begin.

So assuming you’ve set up your project as we’ve specified earlier--that is, an ASPX page that uses a master page with the JavaScript includes for jQuery and jQuery UI as well as placing the jQuery theme information in the ASP.NET theme directory--all you should have left is a bit of HTML and the jQuery call.

The Date Picker works with a standard input field.  Since this is a .NET based blog, we will use an asp.net control instead.  It ultimately renders as an input field.

Date: <asp:TextBox ID="m_textBoxDate" CssClass="dateField" runat="server"></asp:TextBox> 

[](//11011.net/software/vspaste)To turn this into a date picker, use the following jQuery code.

$(function() {
    $(".dateField").datepicker();
});

[](//11011.net/software/vspaste)

To render the date picker

![image](/uploads/2009/07/image2.png "image")

By default the button triggers when you set focus into the input field and goes away when the input field loses focus.

An alternate rendering is to use a button:

![image](/uploads/2009/07/image3.png "image")

You do this by using the showOn specifier

$(function() {
$(".dateField")
    .datepicker({ **showOn:** **'button'** });
});

[](//11011.net/software/vspaste)

You can have the calendar show when either the button is clicked or when the field has focus by using:

$(function() {
$(".dateField")
    .datepicker({ showOn: 'both' });
});

There are other options, events, and methods that you can use to create a rich user experience with your application.  Check out the full documentation at the jQuery UI site.

[http://jqueryui.com/demos/datepicker/](//jqueryui.com/demos/datepicker/ "http://jqueryui.com/demos/datepicker/")

.
