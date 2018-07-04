---
title: 'jQuery, Each() and Async Gets'
tags:
  - ajax
  - async
  - each
  - get
  - jQuery
url: 1640.html
id: 1640
categories:
  - jQuery
date: 2009-12-02 06:50:54
---

![H04K0067](/uploads/2009/12/H04K0067.jpg "H04K0067")

One of the things to keep in mind when using jQuery is that nothing is a blocking call.  Sure, there is a certain sequence to when things operate.  But, to be safe, you should always assume that step two will happen during step one.

Nowhere is this more evident than when retrieving content from a URL and inserting that content in your page.

The temptation is to write code that looks something like this

 $.each(json, function(index, entry)
{
    jQuery.get(entry\['url'\], function(html)
    {
        // insert the HTML here.
    }
}

The problem with this is that jQuery.get is an asynchronous call.  This means that once the get has fired, the each loop will continue.  This can cause all kinds of trouble for you, including having a complete iteration skipped, or if you are doing some kind of concatenation prior to inserting the HTML, having HTML for one iteration showing up in the middle of another.

Not exactly what you had in mind, eh?

But there is a fix.  Use the _ajax_ call instead and specify _async:false_ to force the call to complete before allowing another call.

$.each(json, function(index, entry)
{
    jQuery.ajax({ url:  directory + '/' \+ entry\['url'\] , success: function(html)
        {
            // insert the HTML here.
        }
    }, async: false
});

.csharpcode, .csharpcode pre { font-size: small; color: black; font-family: consolas, "Courier New", courier, monospace; background-color: #ffffff; /\*white-space: pre;\*/ } .csharpcode pre { margin: 0em; } .csharpcode .rem { color: #008000; } .csharpcode .kwrd { color: #0000ff; } .csharpcode .str { color: #006080; } .csharpcode .op { color: #0000c0; } .csharpcode .preproc { color: #cc6633; } .csharpcode .asp { background-color: #ffff00; } .csharpcode .html { color: #800000; } .csharpcode .attr { color: #ff0000; } .csharpcode .alt { background-color: #f4f4f4; width: 100%; margin: 0em; } .csharpcode .lnum { color: #606060; }Note too that using _ajax_ without the _async_: false is the same as just using get.
