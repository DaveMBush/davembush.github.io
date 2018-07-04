---
title: jQuery - Auto Scrolling the Slider
tags:
  - automatic scrolling
  - jQuery
  - slider
url: 936.html
id: 936
categories:
  - jQuery
date: 2009-03-23 06:50:55
---

![B03B0037](/uploads/2009/03/b03b0037.jpg) Question from Yuhsin:

> I would like to make the jquery steps slider to do auto-increment when the window loads. Is there an easy way to trigger the slide to move by itself ?

I'm assuming this question is looking for the content to auto scroll as well.  So let's pull up the [code from last week](/2009/03/12/jquery-using-slider-as-a-scrollbar/) and take a look at this issue. There is a timer plugin available for jQuery that will do some of the work for you, but all we really need for this functionality is:

*   A function to do the auto incrementing
*   A call to the javascript setTimeout() function.

So first, the auto incrementing function.

function scrollWindow() {

    var slideValue;
    slideValue = $("#slider").slider("value");
    if(slideValue > -100)
    {
        $("#slider").slider("value", slideValue - 1);
        setTimeout(scrollWindow, 1000);
    }
}

[](//11011.net/software/vspaste)A couple things to note about this function:

*   We are retrieving the current position of the slider using the .slider() method of the slider widget.
*   We are assuming that the range of our slider is between 0 and -100.
*   We call setTimeout at the end to call this function again in a second.
*   We only call setTimeout if we have not reached the end of our scrolling.

The only thing left is that we need to kick this off when the page loads.  To do this we add a call to setTimeout in our jQuery ready method:

$(function() {
    $("#slider").slider(
    { change: handleChange,
        slide: handleSlide,
        min: -100,
        max: 0
    });
    _**setTimeout(scrollWindow, 1000);**_
});

The actual scrolling of the window happens "automatically" with the code we wrote last week.
