---
title: jQuery Progressbar
tags:
  - javascript
  - jQuery
  - progressbar
url: 1139.html
id: 1139
categories:
  - jQuery
date: 2009-05-20 07:48:00
---

![arct-013](/uploads/2009/05/arct013.jpg "arct-013") Once you venture into the land of AJAX you’ll soon discover the need to let your user know that some work is taking place in the background.  If you can, you’ll want to let them know just how long that work will take before they can continue.  For that, jQuery has the progress bar.

Just like most of the other jQuery UI widgets, you’ll need to make sure you have a theme created.  You can see some of the earlier posts on jQuery UI to find out how to do that.

To use the progress bar, you’ll need some HTML.

<div id="progressbar"></div>

[](//11011.net/software/vspaste)yep.  That’s all you need.  The progress bar will size itself to the containing div.

Next, you’ll need some jQuery code.  For now we’ll go with something simple

$(function() {
    $("#progressbar").progressbar({ value: 37 });
});

[](//11011.net/software/vspaste)

Which will render a progress bar 37% complete

![image](/uploads/2009/05/image3.png "image")

Of course a progress bar is suppose to show some status.  So let’s simulate some status using setTimeout()

$(function() {
$("#progressbar").progressbar({ value: 0 });
setTimeout(updateProgress, 500);
});

function updateProgress() {
  var progress;
  progress = $("#progressbar")
    .progressbar("option","value");
  if (progress < 100) {
      $("#progressbar")
        .progressbar("option", "value", progress + 1);
      setTimeout(updateProgress, 500);
  }
}

[](//11011.net/software/vspaste)Here we are calling the updateProgress function every 1/2 second, retrieving the current value of the progress bar and incrementing it by one if it has not reached 100% done yet.

If you need them, there is an event to handle when the progress bar has been changed and methods to disable, enable, and destroy the progress bar.

It really is a pretty simple control.
