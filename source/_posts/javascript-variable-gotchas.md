---
title: JavaScript Variable Gotchas
tags:
  - hoisting
  - javascript
  - scope
url: 3384.html
id: 3384
categories:
  - Javascript
date: 2016-02-11 08:30:00
---

If you’ve been programming using JavaScript for any length of time, you’ve probably run into several of the JavaScript variable scope gotchas.  You may have even been able to fix them.  But you could prevent these gotchas if you understood better why the gotchas exist in the first place. My goal, through a series of blog post on the topic, is to make us all better JavaScript programmers.  JavaScript is no longer a toy.  Those who survive in the new JavaScript eco system will be those who understand why JavaScript works the way it does. I’m going to approach this topic as a series of puzzles.  This will show the issue and then we can discuss why the issue exist and what to do about it. ![JavaScript Variable Gotchas](/uploads/2016/02/image.png "image") 

Puzzle 1
--------

 1: var a = 'abc';

 2: 

 3: function foo()

 4: {

 5:    console.log(a);

 6: 

 7:    var a = 'xyz';

 8:    console.log(a);

 9: }

 10: 

 11: foo();

Given the code above, what is the value of `a` at line 5 and what is the value of `a` at line 8? (Play Jeopardy music here) OK.  Times up.  What do you think? If you said that line 5 has the value of ‘abc’, you would be wrong.  But I totally understand why you would think that.  I think everyone would agree that the value at line 8 is now ‘xyz’.  So, we will ignore that. Why isn’t the value at line 5 ‘abc’?

### Hoisting

The first thing we need to understand about variables is that no matter where they are declared, the declaration is always ‘hoisted’ to the top of the scope block the variable is declared in.  The assignment happens where we wrote the code. So let’s rewrite our code so that it looks more like what is really happening.

 1: var a = 'abc';

 2: 

 3: function foo()

 4: {

 5:    var a;

 6:    console.log(a);

 7: 

 8:    a = 'xyz';

 9:    console.log(a);

 10: }

 11: 

 12: foo();

Written this way, it is obvious that `a` is either undefined or null at line 6.  Which is it?

### Undefined or Null?

This one always confuses me too.  Mostly because in every other language I work with, if a variable is declared but not assigned, it is almost always null.  The only time it would be undefined is if I had not declared it. In JavaScript things are different. In a strongly typed language, we know more about our variable types when the variable is declared.  So, if we declare a variable as some object type, it is assigned null by default.  But value types are zero’d out.  Not really always null. In JavaScript, we don’t know the type of the variable until it is assigned.  So, all we know when we declare a variable with the var keyword is that there is a variable.  But the type of the variable is undefined.  Therefore, any variable that has not been assigned is going to be undefined.  Not null.

Puzzle 2
--------

Let’s move some code around a bit.

 1: var a = 'abc';

 2: 

 3: function foo(){

 4:    a = 'xyz';

 5: }

 6: 

 7: foo();

 8: 

 9: console.log(a);

What is the value of `a` at line 9? I hope this is an easier puzzle to solve.  Notice that we call `foo()` at line 7, so when we return from `foo()` `a` now holds the value of ‘xyz’ because we didn’t redeclare the variable inside of `foo()`.  Because of variable scoping, `a` assigned the variable that was declared at line 1.

Puzzle 3
--------

Once again, let’s move some code around.

 1: function foo(){

 2:    a = 'xyz';

 3: }

 4: 

 5: foo();

 6: 

 7: console.log(a);

Notice that in this case, we have not declared the variable `a` at all.  We’ve just assigned ‘xyz’ to it inside of `foo()`.  So, what is the value of `a` at line 7? What are our options?

1.  The code won’t run.
2.  a === ‘xyz’
3.  `a` is undefined.

If you were to think that the code won’t run, you would be wrong and you probably haven’t coded with JavaScript very long.  The code will run. So the next question you have to ask yourself is where will `a` be defined?  Since we haven’t declared it, it must automatically get declared some place.  Is that inside of `foo()` or someplace else? The answer is some place else.  The rule is this, if the variable has not been declared in the scope it is being used, the variable is declared as a global variable.  In a browser, this creates a property hanging off the window object. And so, the only valid answer is that `a === ‘xyz’` at line 7.

Don’t shoot yourself in the foot
--------------------------------

As you might imagine, if you aren’t careful, you can really get yourself into a lot of trouble.  Funny thing about computers, they do EXACTLY what they are told.  It really doesn’t matter what you think it should have done. But there is a way to prevent some of the problems above. by adding `"use strict";` as a line in your code.  Many of the common errors that we make while programming in JavaScript will be thrown as exceptions. The other thing you should put in your arsenal is a tool like jsHint which you can get by using WebEssentials in visual studio.  This will tell you when you’ve done things that might not be right. BTW, use jsHint instead of jsLint.  jsLint is WAY too opinionated.  For example, I get Mr. Crawford’s point about forcing the use of a break statement in a switch/case block.  But really!  I should be able to turn it off in the places where not having a break statement is EXACTLY what I want.  jsHint gives you this flexibility.
