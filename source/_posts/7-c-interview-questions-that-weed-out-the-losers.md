---
title: '7 C# Interview Questions [That Weed Out The Losers!]'
tags:
  - 'c#'
  - interview
url: 3195.html
id: 3195
categories:
  - Interview
date: 2015-08-13 07:30:00
---

![image](/uploads/2015/08/image1.png "image")

So, once again, the place I am currently working has been interviewing for some more programmers and we’ve had to laugh at some of the answers we’ve received on some pretty simple question.

For example, in answer to “How do you create an object in JavaScript?”  One applicant responded, “I always use the WHERE keyword.”  What?!!! And that naturally got us all talking about good interview questions.  How can we tell that the applicant is even worth interviewing? The following questions are not meant to be THE interview.  The questions are meant to shorten the interview process by ensuring the applicant has a basic understanding of the language they will be expected to work with.

C# Interview Questions
----------------------

<!-- more -->

### 1\. What is the difference between an Object and a Class?

This is an object oriented 101 question.  So if you can’t answer this, I might try a few other questions for show, but you’ve probably already been counted out.  The way I always described the difference between the two is that the Class is like a cookie cutter and an Object is like the cookie.  The class defines what the object is going to do, but the object is the thing actually doing the work.

A more technical answer would be that the Class defines the object while the Object is the Class active in memory.

### 2\. What is “Polymorphism”?

This is my first stab at making sure you understand the basics of object oriented programming.  Does your answer at least include the concept of virtual functions?  Here is how I explain polymorphism.

Polymorphism gets at the idea that you can have a method in a parent class and a method with the same name in a child class.  If the method in the parent class should be marked virtual and the method in the child class should be marked “overrides.”  At runtime, the decision as to which one is called is based on the type of the object that the method is called from.

*   [http://www.webopedia.com/TERM/P/polymorphism.html](//www.webopedia.com/TERM/P/polymorphism.html)

Some might want to include other concepts

*   [https://en.wikipedia.org/wiki/Polymorphism_(computer_science)](//en.wikipedia.org/wiki/Polymorphism_(computer_science))

But historically, polymorphism has been limited to the basic idea of virtual functions and the applicant's response should, at the very least, reflect this answer.

### 3\. What is the difference between overload, overrides, and shadows?

Again, this is to get at your understanding of object oriented programming generally and the sometimes confusing keywords in the language.

- Overloading gets at the concept that you can have multiple methods with the same name hanging off a given class as long as the methods all have a different signature (parameter types), the code is legal.
- Overrides is how polymorphism is implemented.
- Shadows flips polymorphism on it’s head.  If you mark a method as shadowed using the "new" keyword, then instead of the method getting called based on the object type, the method gets called based on the variable type that is holding the reference to the object.  So, given class A is a parent of class B and both have a method foo() and foo() is marked with the shadows keyword.  If you declare a variable of type A and point that variable to an object of type B, when you call foo off that object, A.foo() will be called.

### 4\. What is the difference between the keyword  “String” and the keyword “string”?

I work with some pretty sharp guys and even they stumbled on this one.  Do YOU know? When I was teaching C# for a training company, I would say, “The only difference is that ‘string’ turns blue in the editor.”  Of course now that you can configure the editor, that’s not really a good answer.  But you get the point.  Both keywords compile down to the same intermediate language.  Technically, “string” is an alias for “String”.  “String” is the proper class.

### 5\. What is “int” an alias for?

Since we’ve already used the term alias by this point, I’m digging deep to find out just how much you know.  The proper answer is that “int” is an alias for the Int32 type.  I can forgive you if you say “class” but it really isn’t a class.  It is a type.

### 6\. What is the difference between a value type and a reference type?

Once again, I’m trying to find out how well you know what is going on.  Do you just hack at your code until it seems to work, or do you really understand what is happening under the hood? Again, when I was teaching this, the explanation always went something like this: The value of a value type occupies memory on the stack and when you do an assignment from one value type to another the data is copied from one memory location to the other.  Each variable is changed in isolation to the other.

A reference type is a variable on the stack that points to memory in the heap that actually holds the value.  When you do an assignment from one reference type to another, only the pointer is copied.  In the end, both variables point to the same location on the heap.

If you change the value of a reference type from one variable, the other variable is impacted with the change because it is the same location in memory you are changing.

The key part of this answer is that the value of a value type occupies the memory the variable represents and that the value of a reference type is pointed to by the memory that the variable represents.  The stack vs heap issue is secondary and in fact gets clouded by the fact that once you put a value type in a reference type, either by boxing or by including it as a member of a class, the whole stack vs heap question gets quite murky.  But, using stack vs heap as a general way of discussing the main issue is a starting point at the very least.

### 7\. What is the primary factor in making code testable?

OK.  You knew I had to stick this one in here, right?  I doubt most programmers have given this much thought so it is OK if they have to spend some time thinking of the answer.

What I’m hoping to hear is that dependencies make code untestable.

Simplistic answer?  Maybe.  But the answer to this question, regardless of if it agrees with mine or not, will tell me if they've had any experience writing unit tests.
