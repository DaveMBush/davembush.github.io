---
title: Why does JavaScript loop only use last value?
tags:
  - closure
  - javascript
  - loops
url: 3937.html
id: 3937
categories:
  - Javascript
date: 2016-06-16 06:30:00
---

You see variations of the question, “Why does JavaScript loop only use the last value?” on StackOverflow all the time.  At work, the guy that sits next to me just ran into the same issue.  And the answer to the question requires a solid understanding of closures and [variable scope](/javascript-scope/).  Something I’ve [written about in the past](/javascript-scope/).  But, when I went back and looked at that article, I was surprised that I had not covered this particular very common topic.

So, here is the basic scenario.  You have some sort of for/next loop that then calls some asynchronous function.  When the function runs, what you see when the code runs is that the last value of the loop index is the value that gets used in the function for every instance that it gets called.

<figure>![](/uploads/2016/06/image-1.png "Why does JavaScript loop only use last value?")<figcaption>Photo credit: [col_adamson](//www.flickr.com/photos/57855544@N00/340654162/) via [Visualhunt.com](//visualhunt.com) / [CC BY](//creativecommons.org/licenses/by/2.0/)</figcaption></figure>

<!-- more -->

An Example
----------

Here is a really simple example that demonstrates the problem.

``` javascript
for(var i = 0;i < 10;i++){
    setTimeout(function(){
        console.log(i);
    },1000);
}
```

But, you will also see it when you try to fire an event.

``` javascript
for(var i = 0;i < 10;i++){
    var img = new Image();
    img.onload = function(){
        alert(testArray[i]);
    }
}
```

Or even more common, when you try to make an AJAX call.

``` javascript
for(var i = 0;i < 10;i++){
    $.ajax({
        url: /* url goes here */,
        success: function (moduleHtml) {
            console.log(i);
        }
    });
}
```

For the remainder of this post, we’ll stick with the first example because the problem is the same and the code for that one has the least moving parts.

The Diagnosis
-------------

The solution to the problem starts with understanding how JavaScript works.  In particular how closures work.  What happens when you use a variable that is declared outside the scope the variable is going to be used in, is that it will use the value that variable has at the time it runs.  It doesn’t get a copy of the value at the time the closure is setup.  If you think of closures as pointers rather than values, maybe that will help.

So, in our working example, when the code actually runs, 10 will get spit out to the console 10 times because by the time the code runs, that is the value that i will have.  Maybe you thought it would be 9.  But the loop stopped looping because i was 10.

If you think, “OK, so I’ll just make the function fire immediately after I set it up by using setTimeout(func,1), let me remind you that in our second example of firing an event, that is essentially what is happening there.  It won’t work either.

Not a Matter of Timing
----------------------

JavaScript has, and probably always will be single threaded.  I say, probably always will be because way too much is relying on the single threaded nature of JavaScript at this point for it to safely change.  If you want to break the web, suddenly change that.

So, even if we could set a timeout value small enough to execute before the loop will complete, what you have to remember about setTimeout and setInterval is that all we are doing when we make those calls is we are saying, “run this code as soon after the timeout value as possible.”  Under the hood it puts the function in the event queue when the timeout value has expired.

Since JavaScript is single threaded, none of this will happen until the code we are currently executing has completed.

Solution 1
----------

One solution is to wrap our code in another closure that will run immediately.

``` javascript
for(var i = 0;i < 10;i++){
    (function(ii){
        setTimeout(function(){
            console.log(ii);
        },1000);
    })(i);
}
```

This example is using an IIFE (Immediately Invoked Function Expression) so that the function runs right away.  The effect is the same as the original code except for now the variable ii is local to our IIFE so it will not change every time the variable i changes.

Solution 2
----------

Now, by this point, you might be thinking, why not just create a new variable ii inside the loop?

``` javascript
for(var i = 0;i < 10;i++){
    var ii = i;
    setTimeout(function(){
        console.log(ii);
    },1000);
}
```

Well, the problem with this is variable hoisting.  Any variable you declare within a function, regardless of where it is declared, is physically declared at the top of the function.  So, you aren’t really creating a variable local to the loop.  You are creating a variable local to the function (or global scope in this case) and you end up with the same problem as before.

But, ES2015 recognizes and has finally provided a means of creating a variable local to a code block rather than just function blocks.  To do this, they’ve introduced the LET keyword.

So, if you change your code to:

``` javascript
for(var i = 0;i < 10;i++){
    let ii = i;
    setTimeout(function(){
        console.log(ii);
    },1000);
}
```

The problem of course is that there aren’t a lot of browsers that support the LET keyword right now.  But there are transpilers that will convert your code from ES2015 to ES5.  And the way they do this is our final solution.

Solution 3
----------

The problem with solution 1 is that while it works most of the time, it really isn’t the most reliable way of solving the problem.  At the very least it sets up a lot more code that we really need.  If we peek under the hood to how the transpilers implement LET, what we see is that they take advantage of the fact that the CATCH block of the try/catch syntax has its own scope.

So, all we have to do is throw i, catch it in the catch block and use the variable we caught in our callback function.

``` javascript
for(var i = 0;i < 10;i++) {
    try{throw i}
    catch(ii) {
        setTimeout(function(){
            console.log(ii);
        },1000);
    }
}
```

It tends to be a bit cleaner than solution 1 and is the solution I prefer.  But, there is another strong reason for using this last solution that most people overlook.  When you wrap a function with an IIFE like we've done with solution 1, it changes the meaning inside the IIFE of `this`, `return`, `break` and `continue`.  Using the `try/catch` mechanism allows you to treat the code as if it were inline with the code the `try/catch` is in.  This is probably closer to what you had in mind when you wrote the original code to begin with.
