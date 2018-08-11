---
title: TypeScript Basics for Angular Developers
tags:
  - angular
  - typescript
url: 4406.html
id: 4406
categories:
  - Angular
date: 2017-08-01 06:30:30
---

For the most part, TypeScript feels a lot like JavaScript.  Most people pick it up without having any formal training.   But, here's the deal.  "Just because you can, doesn't mean you should." The thing that makes me most nervous about Angular is that it is structured so that you can write some really clean code.  But, you don't have to.  Which mean most won't. In fact, recruiters continue to contact me about Angular jobs with rates that make it obvious that hiring an Angular programmer is the same as hiring an HTML "programmer" 10 years ago.  Sorry gang, JavaScript has grown up and so has Angular. So, here are a few things you need to know about TypeScript that will make you a better Angular developer. <figure>![](/uploads/2017/07/TypescriptBasicsForAngular.jpg "TypeScript Basics for Angular Developers")<figcaption>Photo credit: [MIKI Yoshihito. (#mikiyoshihito)](//www.flickr.com/photos/mujitra/8059355303/) via [VisualHunt.com](//visualhunt.com/re/b6d829) / [ CC BY](//creativecommons.org/licenses/by/2.0/)</figcaption></figure>

<!-- more --> 

Variable Declaration
--------------------

There are three ways of declaring a variable in TypeScript.  You can either use the JavaScript ‘var’ keyword like you’ve always done or you can use the ‘let’ keyword or the ‘const’ keyword. But first, what problem are we trying to solve? In the old JavaScript world, we would declare variables in a block of code, but where-ever we declared that variable, the actual declaration was “hoisted” to the top of the function.  In fact, there never was anything like block scope.  Just function scope. This caused one particular problem that was rather common.  If I created a for/next loop that called an asynchronous function, the asynchronous function would use the last value of the for/next incrementer. The primary benefit of using `let` or `const` is that they effectively provide for block level scope so that we can write code like I described above and it will behave in the way we would expect from other languages. Unless you explicitly want to avoid block level scope, you should never use the `var` keyword to declare a variable in TypeScript.  This falls under the “just because you can, doesn’t mean you should” rule. In my experience, you will use `const` more often than `let`.  Here’s the difference. If you are declaring a variable that will only ever be assigned one value, you declare it using `const`.  What isn’t obvious is that changing the contents of an object does not change the value of an object.  So, doing something like this:

**let** myArray = \[\];  myArray.push({});

Would be more valid as:

**const** myArray = \[\];  myArray.push({});

Because pushing something into the array doesn’t change the value, or the pointer, of myArray.  It only changes the content of the array.

Types
-----

The main thing that makes TypeScript what it is, is that it allows us to type-check our code.  You don’t have to.  In fact, there are times when this might get in your way.  But, you have a choice. By default, TypeScript uses inference when it can to figure out the type of a variable.  This is important because I can bet one of the first errors you are going to see is a type mismatch error. You might try to do something like this:

**let** v = 'abc';  // some other code, and then ...  v = 20;

That’s not going to compile. But you could do something like this:

**let** v: any = 'abc';  // some other code, and then …  v = 20;

That is telling the TypeScript compiler that we are OK with the variable v being any type. Other than the Classes and Interfaces that are either part of JavaScript or that we, or a third party, create, there are boolean, number, string, array, enum, any, void, tuple, never, null, and undefined. I’m going to assume that until I mentioned “tuple” you were ok with the types I mentioned.  So, let’s dig a little deeper on those last few. A tuple is a type that wars have been fought over.  OK, well not that bad.  But it is a highly controversial type.  But here is what it lets you do.  It allows you to return a highly defined array or object directly into local variables.  That is, I can specify that a function returns an array or object that has a specific number of elements and each element is a specific type.  It has its uses, but it is probably one of the types that you want to reserve for special cases.  It saves you from having to access array elements or object fields or properties.  That's the long and short of it. The never type allows you to specify that a function never returns.  There are two reasons why this would be true.  First, you’ve entered an infinite loop or second, you’ve thrown an exception.  Again, not something you are likely to use. You can also explicitly specify that a type can only handle null or undefined.  But what is much more likely is that you specify that you don’t want to use these.

Combined Types
--------------

So, let’s say you have a parameter or a variable that accepts multiple types.  You could just use any and go on your merry way.  But, wouldn’t it be nice if you could say, “I want this type to be either a string or a number.”?  Well, you can.  Simply by using the pipe operator between types.

**let** v: string | number;

But we can take this even further.  Let’s say we want to make sure that the variable is a particular class type that we also want to be sure implements a specific interface.  For that, we use the ampersand.

**let** v: Person & Manager;

And while we are at it, what if we want to make sure that a variable only accepts string types that are not null or undefined?  By default, the compiler allows null and undefined to be assigned to anything, but there is a compiler switch that turns that feature off.  If you use the compiler switch and you want to allow null or undefined, you’ll need to use the pipe operator to include them.

Interface
---------

For the most part, a TypeScript Interface looks a lot like an Interface in other languages, but there are some differences that you need to know about. First, you don’t need to create a class that implements an Interface and then instantiate an object from that class in order to have an object of a particular interface type.  Actually, if you stop to think about it, this makes sense.  The problem is, there are a lot of people teaching TypeScript who are still using interfaces this way. But, in JavaScript, you can create an Object Literal.  TypeScript adds to JavaScript.  So, it only makes sense that TypeScript also allows you to create an Object Literal.  So, let’s say you have a parameter that takes an interface of type Name.  As long as the object we pass in conforms to the interface definition, the code will compile.

// interface Name with firstName and lastName as properties  interface Name {   firstName: string;   lastName: string;  }  // function that takes a Name as a parameter  foo(name: Name) {  }  // call the function with an object literal  foo({firstName: 'Dave', lastName: 'Bush'});

Optional
--------

We’ve been talking a lot about Parameters and Interfaces.  In both cases, we often want to define a parameter or a property as optional. For example, most people have a middle name, but our Name interface doesn’t account for that.  If we added it, we’d want to make it optional since it is possible for that to not be included.  On the other hand, we don’t want people added whatever they want. The way we make sure a parameter is optional is by placing a question mark after the property, but before the colon.

interface Name {   firstName: string;  // middleName is optional   middleName?: string;   lastName: string;  }  // foo() now takes an optional name parameter  foo(name?: Name) {}

I would highly recommend that you tweak your tslint rules to require type annotations on all of your code.  Out of the box, the tslint rules that come with the Angular-CLI are a bit too lax in this area.

this
----

The `this` keyword in JavaScript is probably the hardest concept to fully understand.  And while recent advances in the language have helped tame it, it still doesn’t fully conform to the model most people have in their mind of how an Object-Oriented language should behave.  This is because, of course, JavaScript isn’t an Object-Oriented language.  It is a Prototypal language.  There are similarities, but they aren’t they same. TypeScript, on the other hand, is more object oriented.  I say “more” because it is really only object-oriented in the places where you are taking advantage of TypeScript specific features, such as a Class.  If you create an object literal that has an inline function, you are back in JavaScript land. In a class, if you have a method that calls another method in the same class, you must use the `this` keyword to go after it.

**class** **SomeClass** {   someFunction() {   }   someOtherFunction() {  **this**.someFunction();   }  }

This may take some getting used to if you are coming from JavaScript where you can call any function that is in scope without using the this keyword.  But, I can assure you that having this rule imposed on the language solves a lot of bugs caused the “this” side effects, that it is well worth the adjustment.

Arrow Functions
---------------

Fat Arrow functions, Arrow Functions, or Lambda Expressions all refer to the same concept.  They are probably one of my favorite features of the latest version of TypeScript and JavaScript both because they allow me to write code with fewer characters and because they solve a very real problem that has confused JavaScript developers for years. First the problem. If you’ve written any serious application using JavaScript, one of the following scenarios will be familiar to you. Any time you create an event handler, when the function gets called, the ‘this’ keyword isn’t pointing to the object you are in, it is pointing to the context of the event that fired it.  This could be null, a windows object, or something else.  We often get around this problem by using the bind() function to wrap the context of the function. What the arrow functions in TypeScript do is that they form a closure around the current ‘this’ context by taking advantage of how TypeScript is compiled into JavaScript. You see, when your TypeScript code is compiled, every place you referred to ‘this’ it refers to a variable named ‘\_this’.  Inside the arrow function, they refer to this same \_this instead of creating a new one or looking at the context the function was called from. The main difference between a regular anonymous function and an arrow function is that we leave out a lot of junk. **let** newFunc = **function**(x) {  // do something with x  }  Compared to:  **let** newFunc = (x) => {  // do something with x  } But wait! There’s more.  If you only have one line, you can remove the curly braces. Let’s say you want to create an arrow function that returns the square of some number.  You could write this as:

**let** newFunct = (x) => x * x;

Fat arrow functions return the value from the function automatically.

Decorators
----------

I don’t want to spend a lot of time on decorators.  If you’ve been using .NET, you’ll recognized decorators as “Attributes”. Java programmers are probably used to calling them annotations. Effectively, what a decorator does is that it adds additional information to a function, field, or class that marks it for special use.  While you can create your own decorators, we will only concentrate on implementing decorators that have already been defined for us. You’ll know that something is a decorator because it is a symbol prefixed with the at symbol.

Import and Export
-----------------

Last in our discussion of TypeScript are the keywords import and export. But first, why do we need these keywords? Well, if you are familiar with other languages such as C#, VB.NET or Java, you will recognize the concept of Import as the keyword that says, “Tell this file I’m going to reference code from that other file over there in here.”  And then when we compile our code, the compiler makes sure that we are using that other code correctly. A similar thing happens in TypeScript, but in Angular we get the added benefit of also being able to use this information so that we only include the code we are actually using. You see, in the old days, we would suck in entire JavaScript libraries just because we were using a few functions.  But now with concepts like “Tree Shaking” that we will cover later, we can look at the actual code we are referencing and only include that code.  This reduces the size and number of files that our customer has to download to use our applications. The export keyword, on the other hand, tells the compiler what functions, classes, and interfaces external code is able to reference.  If it isn’t exported, it is only available to code in the file it was declared in.

More
----

I have barely scratched the surface of TypeScript here.  There is a lot more available in the language than what I’ve introduced to you here and knowing the parts I’ve left out will ultimately make you a better programmer and make your code more stable.  But the parts I have introduced will get you going and will make you familiar with the parts you will see most often.
