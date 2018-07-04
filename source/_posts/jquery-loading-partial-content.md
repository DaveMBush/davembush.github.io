---
title: jQuery - Loading Partial Content
tags:
  - jQuery
  - load
  - partial content
url: 712.html
id: 712
categories:
  - jQuery
date: 2013-01-29 15:37:54
---

![misc_vol1_087](/uploads/2008/12/misc-vol1-087.jpg) In previous posts, I've covered the core architecture of jQuery.  How it works.  How you call methods.  A brief overview of what's available.

From here on out, we will focus more on specific solutions that jQuery can provide.  One of those is the ability to load partial content from the server and display it back in a section of our web page.

There are several reasons why you might want to do this.  Remember back in the days of frames when we thought a good modular web site meant trying to code the content in frames for the different sections of the web site?  How quickly we found out that didn't work!  To start with, the back button didn't behave correctly, but there were other major problems with this approach as well.

Using jQuery and some DIV tags, you can load content as you need it into the DIV tags using some pretty simple syntax.  

$('#elementId').load('path/to/Content.htm');

That simple line of code will replace whatever is between the open and closing html tags represented by "#elementId with whatever content is in the file represented by 'path/to/Content.htm'.

If you want to, you can filter the data that is returned from the call to load by also specifying a selector:

$('#elementId').load('path/to/Content.htm .OnlyMatchThisClass');

And of course, if the URL is an ASPX page (or other dynamic content) you could pass in parameters on the query string to tell the server what data to retrieve.

You can also send parameters to the server using a post by specifying a second parameter, data, that a set of name/value pairs that will be sent to the server.  You can use this in combination with the serialize() method to pass the contents of form elements in with your request.  This is a step above what the MS-AJAX stuff does.  When you use the default AJAX that comes with ASP.NET, all of the form elements get posted to the server.  With this method, we are able to select exactly which form elements we are sending back and even what ASPX page we might be calling.

Finally, we can pass a third parameter which is a call-back function that will be called by load when it completes.  Note, this is not to say it completed successfully, but only that it did complete.  It will be passed three parameters: the response text, the status, and the XMLHttpRequest object.
