---
title: Cross Browser JavaScript Copy and Paste
tags:
  - copy &amp; paste
  - cross browser
  - javascript
url: 3343.html
id: 3343
categories:
  - Javascript
date: 2016-01-14 08:30:00
---

I've searched all over the Internet for a Cross Browser JavaScript Copy and Paste solution.  I couldn't find anything that really worked.  But I was able to put together the bits and pieces I found into a rather simple solutions. As you may have noticed, I’ve written [quite a few articles about programming in JavaScript](/tags/javascript/) in the past couple of years because I’ve spent most of my time programming in JavaScript.  For the last three months, I’ve been working on a browser based application that needed to be able to copy and paste between the browser and an external spreadsheet.  The main struggle in making this all work correct is that what needs to be copied to the clipboard is the underlying data of the application.

Getting this all working in IE, even the most recent version that runs under Windows 10, was pretty easy.  And fortunately, the browser of choice at this company is Internet Explorer.  But, it is IE 11, which takes twice as long to do anything with JavaScript as Chrome does.  Chrome is their secondary browser and my mission has been to find a way to get copy and paste working reliably using Chrome so that the end user would have a better experience with the application.

While the work I’ve been doing has been using Angular, the solution that I provide in this article using plain JavaScript with no dependency on any framework.  I want the solution to be available regardless of what framework you might be predisposed to use.  If you use angular, or jQuery, or whatever, the code should be easy enough to adapt.

![image](/uploads/2016/01/image-1.png "image")

<!-- more -->

Demo HTML
---------

For the purposes of this article, the HTML we are dealing with looks like this:

``` html
<body >
    <div tabindex="0" id="element1">
        Element To Paste Into
    </div>
    <div tabindex="0" id="element2">
        Another Element To Paste Into
    </div>
    <script src="App/app.js"></script>
</body>
```

The `tabindex` attribute makes the element focusable.  We’ll use the `id` in our JavaScript to bind event handlers to the element.

Demo CSS
--------

This is the CSS that we are using in the code to layout the `DIV`s:

``` css
#element1 {
    width: 49%;
    height: 100px;
    float: left;
    border: 1px solid black;
}

#element2 {
    width: 49%;
    height: 100px;
    float: right;
    border: 1px solid black;
}
```

This is really simple CSS.  Just enough to layout the screen with the `DIV`s side by side.

Trapping CTRL-C and CTRL-V
--------------------------

To start out with, we need a way of trapping the `CTRL-C` and `CTRL-V` keystrokes on the element we want to copy or paste or content into.  This is pretty standard JavaScript that anyone who has worked with JavaScript before should already be familiar with.

``` javascript
function keyBoardListener(evt) {
    if (evt.ctrlKey) {
        switch(evt.keyCode) {
            case 67: // c
                copy(evt.target);
                break;
            case 86: // v
                paste(evt.target);
                break;
        }
    }
}

document.getElementById('element1')
    .addEventListener('keydown', keyBoardListener);
document.getElementById('element2')
    .addEventListener('keydown', keyBoardListener);
```

Internet Explorer
-----------------

You may have noticed that the code above is calling a copy function and a paste function that we have not declared yet.  That is where a lot of the magic happens.

Implementing copy and paste using Internet Explorer is pretty trivial.

``` javascript
function paste(target) {
   if (window.clipboardData) {
        target.innerText = window.clipboardData
            .getData('Text');
        return;
    }
}

function copy(target){
    if(window.clipboardData){
        window.clipboardData.setData('Text',
            target.innerText);
    }
}
```

However, keep in mind that this method has been deprecated.  But it gives us a place to start.

A better way to implement the copy, would be by using newer document.execCommand('copy') because that method generally works well in all three of the major browsers.  

``` javascript
function copy(target){
    target.select();
    document.execCommand('copy');
}
```

Chrome & Firefox
----------------

But the problem with using this method is that in Chrome and Firefox, the copy command only works with elements that are editable.  In chrome the element doesn’t have to be visible and with Firefox it does.  This requires us to change our copy command so that it will work in all three browsers.

``` javascript
function copy(target) {
    // standard way of copying
    var textArea = document.createElement('textarea');
    textArea.setAttribute
        ('style','width:1px;border:0;opacity:0;');
    document.body.appendChild(textArea);
    textArea.value = target.innerHTML;
    textArea.select();
    document.execCommand('copy');
    document.body.removeChild(textArea);
}
```

By making the text area 1 pixel wide, giving it no boarder, and making it transparent, it isn’t really visible to the user and it fulfills the requirements of all three browsers.

But What about Paste?
---------------------

OK.

Getting copy working in a three browsers is pretty trivial.  But, paste, on the other hand, is where all the tricks come into play.

In short, you can’t read from the clipboard during the keyboard event.  However, we can trap the system level paste event.  Sometimes.  The problem is that ‘sometimes’ quirk.

In Chrome the paste event will fire all of the time.  In Firefox, it will only fire when an editable field is on the screen.  Further, the paste event can only be bound to certain elements.  The only one we can be sure will always be available is the window element.  Capturing paste at the window level doesn’t tell us what element we should paste into.

But with a little JavaScript magic we can use the keyboard event handler in combination with a paste event handler to grab the contents from the clipboard and put the data where we want it.

We are going to need to put some variables in place that will hold

1.  A flag that will tell us that the paste event has fired.
2.  A variable to hold the data from the clipboard that we retrieve.

When the paste event fires, we will set these two variables. Meanwhile, back in our paste event, we will setup a timeout function that will look to see if the flag has been set, and if it has it will retrieve the data from the data variable and place it into the DIV.

Oh.  And we have that issue that the paste event will only fire if an editable field is on the screen.  Fortunately, paste fires after the keydown event.  So as part of our keydown event, we will dynamically create a textarea to enable the paste event for Firefox.

So our paste code for Chrome and FireFox looks like this:

``` javascript
function paste(target) {
    function waitForPaste() {
        if (!systemPasteReady) {
            setTimeout(waitForPaste, 250);
            return;
        }

        target.innerHTML = systemPasteContent;
        systemPasteReady = false;
        document.body.removeChild(textArea);
        textArea = null;
    }

    // FireFox requires at least one editable
    // element on the screen for the paste event to fire
    textArea = document.createElement('textarea');
    textArea.setAttribute
        ('style', 'width:1px;border:0;opacity:0;');
    document.body.appendChild(textArea);
    textArea.select();
    waitForPaste();

}

function systemPasteListener(evt) {
    systemPasteContent =
        evt.clipboardData.getData('text/plain');
    systemPasteReady = true;
    evt.preventDefault();
}

window.addEventListener('paste',systemPasteListener);
```

Back to Internet Explorer
-------------------------

I know what you are thinking now.  Why not make Internet Explorer work the same way?  Yeah, I thought the same thing.  But as soon as you take out the lnternet Explorer specific paste code, you get an exception in the paste event handler.  Turns out, the only way the paste event works is if you access the clipboard.getData() method.  At least, that’s how it works today.  So, I’m just leaving that code as it is.

And there you have it.  Cross browser JavaScript Copy and Paste.

Cross Browser JavaScript Copy and Paste Source Code
---------------------------------------------------

A project with the final code is available on [GitHub](//github.com/DaveMBush/CrossBrowserCopyAndPaste).
