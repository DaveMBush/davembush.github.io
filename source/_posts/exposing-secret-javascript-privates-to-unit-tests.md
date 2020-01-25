---
title: Exposing Secret JavaScript privates to Unit Tests
tags:
  - javascript
  - test driven development
  - unit test
url: 3949.html
id: 3949
categories:
  - Javascript
date: 2016-06-23 06:30:00
---

The question comes up all the time, “How do I access JavaScript privates from my Unit Tests?”  And invariably, the purist chimes in with the answer, “you don’t”.

But, isn’t the point of unit testing to allow us to test UNITs?  Why artificially limit our ability to test units if we don’t need to?  If we had the ability to create protected members, wouldn’t we tests those separately? So, what follows is how I surface my private JavaScript members so I can access them during tests without having to make them public during the run of my protection code.

![Exposing Secret JavaScript privates to Unit Tests](/uploads/2016/06/image-2.png "Exposing Secret JavaScript privates to Unit Tests")

Lean on JavaScript
------------------

My JavaScript unit testing framework of choice is Jasmine.  Not so much because it does all I would like it to do or because there isn’t something ‘better’ available but because it has become the defacto standard for unit testing JavaScript and nothing else I’ve seen is significantly better.  There is one part of this technique that is going to lean on the fact that I am using Jasmine, but I’m sure you can adapt it to your testing framework.

But first, let’s review how you would create private JavaScript members in the first place.

Creating Private Members
------------------------

In standard ES5 code, a simple object might be defined using syntax that looks something like this.  Recognize there are multiple ways to create objects and things that look like classes in JavaScript.  What follows is just enough code to get the point across.

``` javascript
function MyClass(){
    function privateMember(){

    }
    function publicMember(){
        privateMember.apply(this);
    }
    this.publicMember = publicMember;
}
```

Note that our privateMember is used by publicMember but is not accessible from the outside.  I’m also using apply(this) to pass the context to the privateMember function.  This may not be necessary if you aren’t using this in the privateMember function and you could use privateMember.bind(this) to make this automatic.  That’s one of the interesting things about JavaScript.  There are always multiple ways to achieve the same goal.  None of them particularly better than the other but some more standard than the other.

Notice that the only thing that actually makes our publicMember public is that I’ve attached the function pointer to this.

Exposing Private for Jasmine
----------------------------

The easiest way I know of to expose the private member variables for Jasmine is to conditionally assign the private members to this if jasmine is defined.

``` javascript
function MyClass(){
    function privateMember(){

    }
    function publicMember(){
        privateMember.apply(this);
    }
    this.publicMember = publicMember;
    if(jasmine){
      this.privateMember = privateMember;
    }
}
```

As long as you don’t use the jasmine global variable for something other than jasmine, this should work.

And now you can test your private functions.

What about Spys?
----------------

If you are testing your private functions on their own, you’ll probably have a need to place spys on them when you test the other functions in your application that call them.  This is where things get just a bit interesting.

If we leave things as they are, and you place a spy on the function that we exposed, your spy will never get called.  The reason for this is because of the way pointers work.

In our example above, our publicMember() function is going to call our privateMember() function regardless of how we manipulate the this.privateMember pointer.  This is because, while the variables are pointing to the same function, they are still two different variables and, because of the way spys work internally, you’ll end up changing the this.privateMember variable without impacting the call to privateMember().

We need to write a little extra code in our if(jasmine) block to make sure that after we’ve exposed privateMember(), the now public version of privateMember() gets call by publicMember() instead of the private version of privateMember().

To do this we are going to need to play “towers of hanoi” with our variables.

``` javascript
function MyClass(){
    function privateMember(){

    }
    function publicMember(){
        privateMember.apply(this);
    }
    var oldPrivateMember;
    this.publicMember = publicMember;
    if(jasmine){
        if(oldPrivateMember){
            privateMember = oldPrivateMember;
        } else {
            oldPrivateMember = privateMember;
        }
        this.privateMember = privateMember;
        privateMember = (function(){
            this.privateMember();
        }).bind(this);
    }
}
```

The gist of what this new code does is that it captures the pointer to the privateMember() into oldPrivateMember.  Once we have that, we can make this.privateMember point to the original privateMember and then make our original privateMember point to a new method that calls this.privateMember, which is what our spy will call if we’ve set one up.

The if(oldPrivateMember) stuff is just protection code to make sure we don’t do this more times than we need and end up calling this.privateMember up the call stack multiple times until we finally get to the privateMember function we ultimately want to call.  Depending on how you implement classes, you may or may not need this code.
