---
title: ASP.NET Dynamic Validator
tags:
  - asp.net
  - 'c#'
  - dynamicvalidator
url: 1613.html
id: 1613
categories:
  - ASP.NET
date: 2009-11-16 07:23:55
---

![misc_vol3_100](/uploads/2009/11/misc_vol3_100.jpg "misc_vol3_100") One of the controls that was added to ASP.NET 3.5 in the SP1 release was the Dynamic Validator control. I completely missed it. What it does is pretty cool.  But it doesn’t really do what you’d think it might.  Or at least not what I thought it would.  “Dynamic” implies to me some kind of hook up to the database.  But the Dynamic Validator control doesn’t hook to the database.  At least not directly.  What it does, however, is a lot more flexible.  You would use the Dynamic Validator control in a lot of the same instances that you’d use the Custom Validator.  While the Custom Validator has a lot more flexibility, the Dynamic Validator is a bit easier to implement. The Dynamic Validator displays any exceptions that the OnChanging event of a field in your datasource.  This allows you to put all of your validations in the OnChanging event of the control rather than an event handler of the Custom Validator. To demonstrate, place a TextBox control and a Dynamic Validator control on your page and set the ControlToValidate property to point to the TextBox control.

<asp:TextBox ID="TextBox1" runat="server"></asp:TextBox>
<cc1:dynamicvalidator runat="server" ID="dynamicValidator" 
   ControlToValidate="TextBox1"></cc1:dynamicvalidator>

You’ll need to set up some sort of databinding between the text box and a datasource because the validation happens at the field level, not at the display level. To implement the validations, create an On_Field_Changing event handler of a partial class that represents the row you’ve bound for the field the TextBox is bound to and create some sort of check.  Throw an exception if the check fails. The validation control will then pick up the exception and display it for you. **Other places talking about the DynamicValidator control** [Passing Arguments to a Dynamic Data Field Template from a UIHint ...](//weblogs.asp.net/craigshoemaker/archive/2008/05/08/passing-arguments-to-a-dynamic-data-field-template-from-a-uihint-attribute.aspx) -  The ASCX is pretty basic. All that is here is my UI control and the needed validation controls. ... Generic Access to ASP.NET Dynamic Data UIHint Attribute Values. Friday, May 09, 2008 1:01 PM by Craig Shoemaker. Yesterday I published a post titled, Passing Arguments to a Dynamic Data Field Template from a UIHint ...
