---
title: jQuery Splitter
tags:
  - asp.net
  - jQuery
  - splitter
url: 1305.html
id: 1305
categories:
  - jQuery
date: 2009-07-21 06:58:20
---

![land-015](/uploads/2009/07/land015.jpg "land-015")

Hey!  This is pretty cool.

I was just mentioning at the last jQuery presentation I did that there were some controls that were definitely missing from the jQuery UI suite of widgets--and then I found this.

This other guy named [Dave](//methvin.com/splitter/) has created it for us.

Here’s how we use it.

The first thing you’ll want to do is download and install the plugin into your js directory and include it on your page.  (You can get the plugin by following the link to Dave.)

<asp:Content ID="Content1" ContentPlaceHolderID="head" Runat="Server">
    <script src="js/splitter-1.5.js" type="text/javascript"></script> </asp:Content> 

[](//11011.net/software/vspaste)

[](//11011.net/software/vspaste)You’ll also want to create your own js file that will be called by the page as we’ve discussed previously in this series.

To get a simple splitter to render on the page, you’ll need two DIV tags, one for the left (or top) and another for the right (or bottom).  You wrap those with another div tag to be the splitter’s container and that’s it.  The rest is jQuery.

<div id="splitThis">
<div>Left Content</div>
<div>Right Content</div>
</div>

[](//11011.net/software/vspaste)Next, you’ll want some stylesheet information to control the size of the container, at the least.

#splitThis {
    width:200px;
    height:200px;
    border: solid 1px black;
}

[](//11011.net/software/vspaste)You’ll also want a class for the splitter.  The two classes that will be available are vsplitbar and hsplitbar (vertical and horizontal).  Since we are producing a left/right splitter, we will create a class for vsplitbar.

Now the jQuery to turn this into a splitter

$(function() {
    $("#splitThis").splitter();
});

Don’t forget to assign the ASP.NET theme to your page if you are using themes for your CSS or it will not render correctly.

And here’s the result.

![image](/uploads/2009/07/image8.png "image")

Dave has a lot of other demos and some great documentation on how to use this control as well as great documentation on places it isn’t quite working yet.  Overall, it’s a great control and I recommend it to you.
