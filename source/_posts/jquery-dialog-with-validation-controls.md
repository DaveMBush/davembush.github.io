---
title: jQuery Dialog – With Validation Controls
tags:
  - asp.net
  - dialog
  - jQuery
  - validation controls
url: 1206.html
id: 1206
categories:
  - jQuery
date: 2009-06-25 06:55:00
---

![sahara](/uploads/2009/06/sahara.jpg "sahara")

Chances are, you’ll eventually want to use a dialog box in combination with some form elements, and when you do, you’ll probably want to implement some validation.

True, there are some great validation routines available in jQuery, but they only validate on the client side.  They are, after all, Javascript.

<!-- more -->

As you are probably aware, the advantage of using validation controls with ASP.NET is that they validate on both the client side and the server side and even if we assume that everyone using our web page is using Javascript and has it turned on, there is still the possibility that someone will turn Javascript off so they can circumvent your validations.

So how do we use validation controls AND allow the form to be a jQuery dialog?

First, let’s set up a simple web form that we can turn into a dialog.  I suggest using a panel control to put all of our form elements in so that we can set the default button.  We will then wrap the panel in a DIV tag that will become our dialog.

Here is our main HTML for the form

``` html
<div id="dialog" title="Enter your name">
<asp:Panel DefaultButton="Button1" ID="Panel1" runat="server">
    <asp:Label ID="Label1" runat="server" Text="Name:"></asp:Label>
    <asp:TextBox ID="TextBox1" runat="server" ValidationGroup="Dialog"></asp:TextBox>
    <asp:RequiredFieldValidator ID="RequiredFieldValidator1" runat="server" ControlToValidate="TextBox1" Display="Dynamic" ErrorMessage="Enter your name" ValidationGroup="Dialog">
    </asp:RequiredFieldValidator>
    <br />
    <asp:Button ID="Button1" runat="server" Text="Button" ValidationGroup="Dialog" />
</asp:Panel></div>
```

There are a couple of things to note:

1.  I’ve used the DefaultButton property to tell .NET that Button1 should fire if I’m in a control inside this dialog and I press the enter key.  You’ll need this if the page has other forms on it.
2.  I’ve also set the ValidationGroup for all of the appropriate controls on the page.  This is particularly important.  If you have other buttons that cause a postback that are not in the dialog, it will still try to run the validations because, as far as .NET is concerned the validation controls are visible and should be run.  By making them part of a validation group they will only fire when the Button1 control causes the post back.

Next, we’ll want to turn this all into a dialog.  We do that in our Javascript file using the familiar syntax

``` javascript
$("#dialog").dialog({ modal: true,
    bgiframe: true });
```

As long as all of your validation controls use client side validation, this should be all you need.
