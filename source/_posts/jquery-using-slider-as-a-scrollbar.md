---
title: jQuery - Using Slider as a Scrollbar
tags:
  - jQuery
  - scrollbar
  - scrolling content
  - slider
  - ui
url: 906.html
id: 906
categories:
  - jQuery
date: 2009-03-12 08:23:18
---

![misc_vol3_074](/uploads/2009/03/misc-vol3-074.jpg) Last week I introduced the jQuery UI element known as a slider and indicated that this control could easily be used as a custom scrollbar. Today I'm going to show you exactly how that's done. First we need to adjust our HTML.  We need to add a content area to the HTML we had last week.  I'm going to put this content area to the right of the scrollbar, but you could put it to the left by simply changing the CSS.

    <div id="slider"></div>
    <div id="scroller"><div id="content"> here is lots of text<br />
    </div></div> 

The CSS to position this all correctly looks like this:

#slider {
    height: 242px;
    width: 13px;
    margin:0px 10px 0px 10px;
    float:left;
}
#scroller {
    width: 1159px;
    height: 243px;
    overflow:hidden;
}
#content {
    width: 215px;
}

Not much exciting here.  Slider is basically the same as last week.  The area we want to scroll is composed of two elements, a div we've given the ID "content" that is nested inside another div we've named "scroller."  The scroller is a fixed height and set so that if the content is longer than the scroller, it will not be displayed. The rest is simple jQuery magic:

$(function() {
$("#slider").slider(
    { change: handleChange,
        slide: handleSlide,
        min: -100,
    max: 0 });
});

function handleChange(e, ui) {
    var maxScroll = $("#scroller")
      .attr("scrollHeight") -
      $("#scroller").height();
    $("#scroller")
      .animate({ scrollTop: -ui.value *
     (maxScroll / 100)
    }, 1000);

}

function handleSlide(e, ui) {
    var maxScroll = $("#scroller")
      .attr("scrollHeight") -
      $("#scroller").height();
    $("#scroller")
      .attr({ scrollTop: -ui.value
        \* (maxScroll / 100)   });

}

At the top of our Javascript, we are simply wiring up our slider control.  The most notable thing you'll want to pay attention to here is that I've set the minimum height and maximum height so that zero is at the top of the slider.  If you don't do this, the slider will start with the thumb at the bottom.  Not exactly what we have in mind. We need to wire up two event handlers, slide and change, so that the content area will scroll as we move the thumb.  We've done that in the document-ready function. The two event handlers first determine the maximum scroll height and then position the content area within the scroller area.  Again, to make this work correctly, we needed to negate the value that was passed in in the ui parameter. Obviously, it would not take a whole lot of work to package this all up as a plugin that does all the work for you, but it helps to know how to do it if you need to.
