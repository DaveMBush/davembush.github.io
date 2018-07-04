---
title: Host jQuery at Google (with Intellisense support)
tags:
  - google
  - intellisense
  - jQuery
  - visual studio
url: 647.html
id: 647
categories:
  - jQuery
date: 2012-12-04 11:03:02
---

![tp_vol3_007](/uploads/2008/12/tp-vol3-007.jpg) While reviewing my RSS feed this morning, I found this article: [3 reasons why you should let Google host jQuery for you | Encosia](//encosia.com/2008/12/10/3-reasons-why-you-should-let-google-host-jquery-for-you/) I had no idea! The three reasons are:

1.  Decreased Latency Google will serve the data from the closest server
2.  Increased Parallelism More threads are available to download content specific to your application instead of downloading this common library.
3.  Better caching They may already have the library on their computer.

Here is the code you should be using to include jQuery in your application to use the Content Delivery Network at Google:

    <script type="text/javascript" src="http://ajax.googleapis.com/ajax/libs/jquery/1.2.6/jquery.min.js">
    </script> 

[](//11011.net/software/vspaste)The one issue you may have to deal with is that if you are using the intellisense files for Visual Studio, you will need to find some alternate method.  Here's one: First, place the reference to your local jquery files like we've been doing:

<script src="js/jquery-1.2.6.min.js" type="text/javascript"></script> 

[](//11011.net/software/vspaste)But then in the onload event, put in code to replace the src attribute so that it will get the js file from google:

protected void Page_Load(object sender, EventArgs e)
{
 foreach (Control c in Page.Header.Controls)
 {
  if (c.GetType() == typeof(LiteralControl))
   ((LiteralControl)c).Text =
    ((LiteralControl)c).Text.Replace(
    "src=\\"js/jquery-1.2.6.min.js\\"",
    "src=\\"http://ajax.googleapis.com/ajax/libs/jquery/1.2.6/jquery.min.js\\"");
 }

}

[](//11011.net/software/vspaste)This code might be optimized a bit better.  For example, by making sure the js reference is the first element in your header, you could skip the foreach loop and just grab the first element out of the controls collection.
