---
title: Dynamically Change class Attribute From ASP.NET
tags:
  - asp.net
  - class
  - dynamic classing
  - html
url: 1445.html
id: 1445
categories:
  - ASP.NET
date: 2009-09-29 05:25:00
---

![B03B0015](/uploads/2009/09/B03B0015.jpg "B03B0015") I recently received a question from another programmer I know who's been using PHP prior to ASP.NET that made me think harder about a problem we've all had in ASP.NET.  The basic problem is this:

<!-- more -->

How do you dynamically change the class of a hyperlink based on the page name so that the hyperlink that represents the current page is styled differently than all the other hyperlinks on our screen?  If you want you can substitute any other HTML element you want, but the problem remains the same.

The first, most obvious answer would be to create a case statement of some sort.

``` csharp
string fileName = AppRelativeVirtualPath
    .ToLower().Replace("~/","").Replace("/","_")
    .Replace(".aspx","");
switch(fileName)
{
     case "fileone":
             m_hyperLinkFileOne.CssClass =
                "selectedClass";
             break;
    // etc...
}
```

But we all know just how ugly that code will get once we start adding pages to our site.   Yuck!  Unfortunately, this is exactly the code I've been recommending up until today.

What was I thinking?!

If you are familiar with ASP.NET at all, you should already be familiar with the Page_Load event handler.  I bet that's where you do 90% of your page initialization.  But did you know that there is a load event that fires for each control?

Further, you can have all of your controls point to the same load event handler.  So if we take advantage of this, we can write very tight code that automatically sets the class based on the current page name.

``` csharp
// This code assumes that the ID of the hyperLink
//  controls follow
// the form of m_hyperLinkDestinationPageName
// ie, a link to default.aspx would become
// m_hyperLinkDefault
void HyperLinkLoad(Object sender, EventArgs args)
{
    // Only need to do this once if you have ViewState
    // enabled.
    if(!IsPostBack)
    {
        string fileName = AppRelativeVirtualPath
            .ToLower().Replace("~/","").Replace("/","_")
            .Replace(".aspx","");
        // sender will always point to the control that
        // fired the event so, assuming that only
        // asp:hyperlink controls call HyperLinkLoad
        if("m_hyperlink"+file ==
           ((HyperLinkControl)sender)
           .ID.ToLower())
           ((HyperLinkControl)sender).CssClass=
              "selectedClass";
    }
}
```
