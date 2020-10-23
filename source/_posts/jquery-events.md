---
title: jQuery - Events
tags:
  - events
  - javascript
  - jQuery
url: 625.html
id: 625
categories:
  - jQuery
date: 2013-11-06 15:45:24
---

As well as being able to change the class associated with an element or a set of elements on a screen, jQuery also allows you to fire events.  You might want to do this, for example, if you want to simulate the clicking of a button. Of course, if you are going to fire an event, you'll probably need some sort of event listener setup to handle that. We will address firing events first since it has the least amount of code needed. All you need to do is select the element or elements using the selectors we've already discussed and then call the method trigger('eventname'). So, to click a button, your code might look something like this:

<!-- more -->

``` javascript
$("#main").trigger('click');
```

Since the click method is so common, jQuery has a shortcut method, click(), that does the same thing, so we can rewrite our code as:

``` javascript
$("#main").click();
```

But if we click a button, we probably want some JavaScript to execute because of it.  This is where jQuery makes life much easier for the JavaScript programmer. If you are familiar with JavaScript you are probably most familiar with attaching code to your HTML elements by using the on____ attributes.  To attach a method to the click event of the anchor tag you might write something like:

``` html
<a href="#" onclick="method();" >text</a>
```

If you were particularly clever, you might do some sort of event binding using code:

``` javascript
document.getElementById('anchorId').onclick = functionName;
```

But even doing that we are left with the ugly potential of having already assigned a function to the element, in which case this code would overwrite it. In fact, if you've been coding JavaScript for a while, you are probably quite familiar with the problem of needing to add an event handler to the onload event of the document only to find out that doing so wiped out some critical JavaScript code that was already assigned to that event. You'll be happy to know that assigning code to an event in jQuery is both simple and non-destructive.

``` javascript
$('#anchorId').bind('click',function()
{ //code goes here })
```

This code would normally be placed inside the ready handler that you've seen in previous posts so that your code would ultimately look something like this:

``` javascript
$(function() {
    $('#anchorId').bind('click', function()
    { //code goes here })
});
```

And since the click event is so common, we can replace `bind('click',function...)` with `click(function()....)`

``` javascript
$(function() {
    $('#anchorId').click( function()
    { //code goes here })
});
```

And if someone decides to attach another method to the same click event, both methods will run.
