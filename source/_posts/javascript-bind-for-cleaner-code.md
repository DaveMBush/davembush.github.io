---
title: JavaScript bind() for cleaner code
tags:
  - bind
  - callbacks
  - currying
  - events
  - javascript
url: 3963.html
id: 3963
categories:
  - Javascript
date: 2016-07-06 06:30:00
---

Several weeks ago, I wrote about [how closures impact calling asynchronous functions](/why-does-javascript-loop-only-use-last-value/) in a loop and several ways of dealing with that problem. In my recent coding, I’ve discovered an even more simple way of dealing with this problem. In the process, it removes the anonymous function and eliminates the linting error, ‘Don’t make functions within a loop’ You see, I’ve been experimenting with JavaScript `bind()`. And as it turns out, we can use bind in multiple situations, including dealing with the closure issue I mentioned a couple of weeks ago. <figure>![](/uploads/2016/07/image.png "JavaScript bind() for cleaner code")<figcaption>Photo credit: [Connor Tarter](//www.flickr.com/photos/connortarter/4754231502/) via [VisualHunt](//visualhunt.com) / [CC BY-SA](//creativecommons.org/licenses/by-sa/2.0/)</figcaption></figure>

<!-- more --> 

What Is bind()?
---------------

The bind function is a recent addition to the JavaScript spec. So this will only work on recent browsers. You can [check the compatibility table](//kangax.github.io/compat-table/es5/) (for all things JavaScript) to see which browser implement `bind()` as well as other JavaScript features. I looked over the list and there aren’t any browsers that don’t support `bind()` that I care to support, so I’m good. Your mileage may vary. What bind does is that it automatically wraps your function in its own closure so that we can bind the context (the this keyword) and a list of parameters to the original function. What you end up with is another function pointer. https://gist.github.com/DaveMBush/d258dbdc23c846fde767ad6143771a53#file-bind1-js Notice that we not only bound this to the `foo()` function, but we also bound the two parameters. So, when we call `newFoo()` the return value will be 7. But what happens if we change the parameters before calling newFoo?

Changing bind parameters
------------------------

If we bind parameters to `foo()` using variables and then change the variables prior to calling `newFoo()`, what do you expect the value to be? https://gist.github.com/DaveMBush/d258dbdc23c846fde767ad6143771a53#file-bind2-js The return value is still going to be 7 because `bind()` binds the value of the parameters, not the actual variables. This is good news and, like I said, we can use this to great advantage in our code. But where I think it will display the most usefulness to me is in my call backs

Bind and callbacks
------------------

You should remember from that article that one of our solutions to dealing with callbacks in loops was to create an anonymous function around the function we wanted to call. https://gist.github.com/DaveMBush/79dcd8c1fa905d1720dd0798ba6fac18#file-whydoesjavascriptlooponlyuselastvalue-sol1-js But we can greatly simplify this code by using bind instead. https://gist.github.com/DaveMBush/d258dbdc23c846fde767ad6143771a53#file-bind3-js We can do this because each call to bind gives us a new function pointer and the original function remains unchanged. Meanwhile, we also remove the linting error ‘Don’t make functions within a loop’ because we aren’t creating the function in a loop, we are just pointing to a function that was created outside of the loop.

Bind for Event Handlers
-----------------------

Another place where `bind()` will clean up your code is with event handlers.  Everyone knows, or should know, that when an event handler is called, the context it is called on is the thing that generated the event and not the object that the event handler was created in.  But, by using bind, you can be sure that the function is being called on the correct context. https://gist.github.com/DaveMBush/d258dbdc23c846fde767ad6143771a53#file-bind4-js Not that you would write your code exactly like that, but that is just to get the point across.

Currying
--------

What?! OK.  I’ll be the first to admit that my functional programming knowledge is limited.  But the best definition of Currying I can give you is that it allows you to pass parameters to function in multiple steps. Using binding, we achieve Currying by writing code that looks something like this: https://gist.github.com/DaveMBush/d258dbdc23c846fde767ad6143771a53#file-currying-js
