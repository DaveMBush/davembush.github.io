---
title: 'jQuery, bgiframe and IE6 z-order hacks'
tags:
  - bgiframe
  - hack
  - ie6
  - iframe
  - jQuery
  - select
url: 839.html
id: 839
categories:
  - jQuery
date: 2009-02-19 05:19:38
---

![tp_vol3_006](/uploads/2009/02/tp-vol3-006.jpg) Well, it probably looks like an odd title.  But let's face it: we still have to support IE6 for some of our clients no matter how much we don't like it or it shouldn't be.  And one of the most frustrating items that we ever had to support in IE6 is the fact that certain controls and plug-ins don't obey z-order. What this means in a practical sense is that if we have a menu layer that drops over one of these misbehaving bad boys, we have to find some hack around it.  The problem I had to solve is that the code for this project is done.  There isn't a lot of room for tweaking.  The menu we are using is something that is not going to be modified easily.  So, any solution I came up with had to "plug in" to what we already had. Sounds like a job for jQuery to me. I did find a solution that claimed it would do the trick: [bgiframe](//plugins.jquery.com/bgiframe/).  But of course you notice that qualifier, "claimed."  Yes folks, I had trouble getting it to work. What exactly does bgiframe do?  It scripts the [iframe hack](//weblogs.asp.net/bleroy/archive/2005/08/09/how-to-put-a-div-over-a-select-in-ie.aspx) into your popover layer. You see, our menu system is based on a styled, unordered list.  When I told bgiframe to grab the UL and put the iframe in it, it just didn't work.  Oh, it put the iframe in.  But the select windows still showed through. What was odd was that even the iframe was transparent.  Odd, I thought.  It isn't supposed to be transparent. So after half a day of searching, looking at the source for bgiframe, and generally pulling my hair out, I finally remembered seeing this little "hack" for the src property of the iframe.  It's actually noted in the link to the iframe hack I have above. The iframe code should look like:

<iframe src="BLOCKED SCRIPT'&lt;html&gt;&lt;/html&gt;';" scrolling="no" frameborder="0" ...

But the script that bgiframe produces looks like:

<iframe src="javascript:false;" scrolling="no" frameborder="0" ...

the src="javascript" thing works most of the time.  In fact, I kind of got everything working by putting the iframe arount the LI tags instead of around the UL tag.  Only I had some other issues when I did that, and it is a sub-optimal way of getting this all working.  Hack of a hack. Now, I fixed this by changing the bgiframe source.  But if you don't want to be that intrusive, you can just call bgiframe using the src parameter.

$("ul").bgiframe({ src: "BLOCKED SCRIPT'&lt;html&gt;&lt;/html&gt;';" });
