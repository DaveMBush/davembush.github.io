---
title: jQuery - Retrieving HTML Fragments
tags:
  - asp.net
  - jQuery
  - tooltip
url: 775.html
id: 775
categories:
  - jQuery
date: 2013-11-20 16:54:21
---

![sunset-bird](/uploads/2009/01/sunset-bird.jpg) A couple of weeks ago I mentioned that I had built a [tooltip using jQuery](/2009/01/06/jquery-positioning-elements/).  We focused mostly on the positioning of the tooltip at the time because, historically, that's where most of the work has been. But there are other time-saving features that also make the tooltip code I wrote a lot more flexible.  For example, in the past, the javascript code we wrote for tooltips often required us to place the HTML for the tooltip in the HTML page we wanted the tooltip on.  This has two drawbacks. First, if you need the tooltip on multiple pages, you need to include the HTML on multiple pages.  You could, of course, use master pages in ASP.NET or includes of some sort in other web development languages to get around this, but the fact remains that you still need to do this. Second, the HTML is not as modular as it might be.  This isn't as big of an issue, since we could argue that most of our HTML is not as modular as it might be, but when I show you how you can overcome this with jQuery, I think you will recognize the improvement. When I sat down to develop this tooltip, I created an HTML page that has the HTML for the tooltip, and nothing else, in it.  This allowed me to concentrate on the HTML and forget about all the javascript issues.  Once I had the tooltip looking the way I wanted, I copied out the HTML fragment that represented the tooltip, leaving out the HTML that had the head tag, body tag, and other code I needed to make the HTML page an HTML page.  I pasted this code into another HTML file, but this time the HTML file ONLY has the fragment. The other thing you'll want to do is to dynamically create the positional DIV that you will use to hold the tooltip.  This is another place where we typically had to put the content in our HTML page.  Instead we will put this information in the jQuery script.  While I could have included this in the fragment, doing it this way actually makes coding the tooltip easier:

$('<div id="tooltip"></div>').appendTo('body');
$('#tooltip').hide().load('js/ToolTip.htm');

[](//11011.net/software/vspaste)Two lines of code to get our shell of a tooltip in the page. The first line creates the HTML fragment and appends it to the end of the elements inside the body tag. The second line retrieves the DIV element we just inserted, hides it, and then loads the HTML fragment that represents our tooltip. Finally, to get content into the tooltip, we use the HTML property.  I created a class called toolTipCenter that is the location where the tooltip content will go.  To insert content for the tooltip, all you need to do is:

$('#tooltip .toolTipCenter').html('Tooltip Content Here.');

[](//11011.net/software/vspaste)
