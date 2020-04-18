---
title: '7 JavaScript Interview Questions [To Weed Out Imposters]'
tags:
  - interview
  - javascript
url: 3207.html
id: 3207
categories:
  - Interview
date: 2015-08-20 07:30:00
---

![image](/uploads/2015/08/image2.png "image")

Last week, I posted [7 interview questions for C# programmers.](/7-c-interview-questions-that-weed-out-the-losers/)  I guess I forgot that people can’t see in my brain.  So, let me be very explicit this time.  The “weed out the losers” questions are meant to do just that.  Weed out people who have absolutely no business even applying for the job.

You would be amazed at how many people interview for a job who have all kinds of cool buzzwords on their resume, but when you ask them about it, they know nothing about the subject.  I’m not sure if this is the recruiter who is representing them trying to help them out by beefing up the resume to get them in the door or if they actually do this themselves.  But, as someone who interviews, I have to have a way of making sure the applicant I’m interviewing is worth interviewing in the first place.  Hopefully, I can do this over the phone in less than a half an hour.

So, with that said, if your favorite question isn’t on this list, it is probably because it is a question I would save for some future interview.

Also, to those of you who may think that a technical interview doesn’t really tell you if the programmer is a good programmer or not, I have this to say… You are right.  And when I was a younger programmer and was being interviewed with technical questions, I felt the same way.  But now that I’m on the other side of the table, I find that way too often, the people who can pass a technical interview are a lot more likely to be good programmers than the ones who can’t.

And finally, I would not rule out an applicant because he got a couple of questions wrong, or didn’t answer them exactly the way I expected.  But if couldn’t answer most of them, that would raise a huge red flag! So, here are 7 JavaScript Interview Questions you should ask first.  Otherwise, you may be wasting your time.

What are two ways you might create an object in JavaScript?
-----------------------------------------------------------

<!-- more -->

This is a pretty simple question if you’ve been using JavaScript.  Shoot, you should even be able to guess at one of the answers.  But, still, in my experience, there are a lot of people who say they are JavaScript programmers who don’t know how to answer this question.

*   Use the “new” keyword to call a function.
*   Use the open/close curly braces.

``` javascript
var o = {};
```

You might want to follow up with, “using the new keyword, at what point is the object created?”  But, as far as questions that weed out, I would consider that something that could wait until I’m really interviewing to discover how deep the applicant’s knowledge goes.

How would you create an array?
------------------------------

This is a similar level question to “How do you create an object.”  And yet, there are some who can answer the first who can’t answer this one.

While it is possible to create an array with

``` javascript
var myArray = new Array();
```

It is a long way of creating an array.  One would hope that the answer included using square brackets.

``` javascript
var myArray = [];
```

Once again, there are other questions we could ask, but since we only want to know that the applicant is worth further investigation, I would leave the array questioning here.

What is variable hoisting?
--------------------------

This one is a slightly harder question to answer, and I wouldn’t expect anyone to be able to answer this.  But again, we are trying to get a quick determination of the applicant’s skill level.  How well do they understand the language they claim to know? Variable hoisting is the term that refers to the fact that no matter where a variable is declared in a scope, the JavaScript engine will move that declaration to the top of the scope.  If you declare a variable in the middle of a function, for example and assign a value to it all in one line:

``` javascript
function foo()
{
    // a bunch of code here
    var a = "abc";
}
```

The code that actually gets run would look like this:

``` javascript
function foo()
{
    var a;
    // a bunch of code
    a = "abc";
}
```

What are the dangers of global variables and how do you protect against it?
---------------------------------------------------------------------------

The danger of global variables is that someone else could create a variable with the same name and overwrite the variable you are using.  This is a bad idea in any language.

You prevent this in a number of ways.  The most common would be to create one global variable that all of your other variables live in:

``` javascript
var applicationName = {};
```

And then any time you need to create a global variable, you attach it to that object.

``` javascript
applicationName.myVariable = "abc";
```

The other way you can guard against this is by wrapping all of your code in a self-executing function so that any variables that are declared are declared within that function’s scope.

``` javascript
(function(){
   var a = "abc";
})();
```

In reality, you’ll probably end up using both methods.

How do you iterate through members in a JavaScript object?
----------------------------------------------------------

``` javascript
for(var prop in obj){
    // bonus points for hasOwnProperty
    if(obj.hasOwnProperty(prop)){
        // do something here
    }
}
```

What is a closure?
------------------

A closure is what allows a function inside the scope of another function to still be able to see the variables declared in the outer scope even after everything else about that scope has disappeared.  Bonus points if they state something about the dangers of using a closure inside of a for/next loop without declaring a variable to hold the current value of the iteration variable.

Describe your experience unit testing JavaScript
------------------------------------------------

Here we are just trying to find out if they’ve even done unit testing with JavaScript.  It is an open-ended question with no particular right answer but it should tell you something in the process.
