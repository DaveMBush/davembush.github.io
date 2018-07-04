---
title: Property Based Testing in Angular with jsVerify
tags:
  - angular
  - jasmine
  - property based testing
url: 4564.html
id: 4564
comments: false
categories:
  - Angular 2
date: 2018-02-06 06:30:58
---

Several weeks ago, I mentioned that I've been playing around with [Property Based Testing](/property-based-testing/).  In particular, I've been using it with my Angular code.  The framework I've chosen is [jsVerify](//github.com/jsverify/jsverify) because it seemed like the most straight forward of the available tools and it has a documented way of integrating with Jasmine, which Angular test use by default.  Angular with jsVerify.  How does that work? The documentation for how to use jsVerify seems to be written for people who already understand Property Based Testing from some other environment.  This makes picking it up and using it awkward at best. Here's what I've learned along the way. <figure>![](/uploads/2018/02/2018-02-06.jpg "Property Based Testing in Angular with jsVerify")<figcaption>Photo credit: [Official U.S. Navy Imagery](//visualhunt.com/author/a3b62c) on [Visual Hunt](//visualhunt.com/re/d44953) / [ CC BY](//creativecommons.org/licenses/by/2.0/)</figcaption></figure>

<!-- more --> 

The Basics
----------

To install jsVerify into your dev environment, use: `npm install --save-dev jsverify` To use the code in your spec file, import jsverify using: `import * as jsc from 'jsverify';` We us 'jsc' because jsVerify originated from [jsCheck](//github.com/douglascrockford/JSCheck).  Why not just use jsCheck?  Well, because it looks even less well documented.  That doesn't mean that it is, but that is how I felt when I went to the sites and I couldn't be bothered to wade through the wall of text the documentation site presented me with.

A Simple Test
-------------

Now to setup a simple test.  We won't really test anything.  I just want to show the structure of the test. There are two methods you might use that seem very similar.  `assertForall()` and `checkForall()`.  What I didn't realize at first is that `assertForall()` is the one you want to use because it will throw the exception that Jasmine is listening for so that I knows the test failed.  If you use `checkForall()` the test will fail, but Jasmine will think it succeeded.  And if you're thinking, yeah but you should use expect() with checkForAll(), that doesn't always work quite the way you would expect.  No pun intended. The basic structure of a test will go inside of your `it()` block.

``` typescript
describe('Any two numeric values', () => {
  it('should equal 20', () => {
    jsc.assertForall(jsc.integer, jsc.integer, 
      (a: number, b: number) => {
        const a_and_b_equal_20 = a + b === 20;
        return a_and_b_equal_20;
    });
  });
});
```

This test will, obviously, fail.  A test we would expect to pass would look like this.

``` typescript
describe('Any two numeric values', () => {
  it('should be able to be added in any order', () => {
    jsc.assertForall(jsc.integer, jsc.integer, 
      (a: number, b: number) => {
        const a_and_b_equal_20 = a + b === b + a;
        return a_and_b_equal_20;
    });
  });
});
```

You may have guessed by now that `assertForall()`takes a variable number of parameters.  The last parameter is a callback that runs our test.  The parameters before the callback describe the kinds of parameters that will be passed to the callback.  The description of the parameter is of type Arbitrary.  So, what we've said above is something to the effect of, "generate two random integers and pass them to the callback."  You can check the jsVerify site for "Primitive Arbitraries" to see what is built in.

Adding Complexity
-----------------

It won't be long before you run into a situation where the primitive arbitraries won't do the job for you and you'll need to resort to the combinators.  This allows you to create a brand new arbitrary by combining primitives together.  The one I found myself using the most was `oneof()` where you pass a list of arbitraries as an array and the system will pick from the list and generate a new random value from the list.  Don't confuse this with `either()`.  I've used `oneof()` in combination with `constant()`for cases where I've needed to create a random value from a list of possible values. Where things really got interesting though was when I needed to create an object with random values for the properties.  For this, you'll need to use a record.

``` typescript
const recordArb = jsc.record({
  firstName: jsc.string,
  lastName: jsc.string,
  arrayThing: jsc.array(jsc.record({
    fieldOne: jsc.integer,
    dateField: jsc.datetime
  }))
});
```

This will let us pass random objects to our tests.  This is great for testing Reducers.  You'll notice we even were able to create a nested array.  This will create a random length array with random records inside of it.

Arbitraries from Generators
---------------------------

Now that I've been working with it for a while, I can't remember why it was so difficult.  But the one place I did have some trouble was the concept of Generators vs Arbitraries.  Arbitraries are what we need to pass into `assertForall().` Generators are what we use when we need to come up with some way of creating our own special random data.  You rarely need to use this, but when you do, being able to convert the Generator to an Arbitrary will become critical. To convert a generator to an arbitrary, use bless.

``` typescript
jsc.generator.bless(generatorThing);
```

Typing
------

As of this writing, the typing for `checkForall()` is incorrect.  It is typed as returning `Result<any>`when it in fact returns `Result<any> | boolean`I just discovered this so I haven't entered a pull request that will fix the issue.  If you decide to use `checkForall()` instead of `assertForall()`, you'll need to fix up the typings yourself.

Puzzles
-------

The one thing I'm still trying to figure out is the best way of running the test.  The fact of the matter is that jsVerify tests do not lend themselves to the structure of a Jasmine test.  And since I have to generate 100 instances of random data for each test, it may not be efficient to separate each test out into separate it statements. For now, I'm running all related evaluations within one it statement and using the back-tick string delimiter to allow me to have a multi-line it() string that describes all that I'm testing.  But, by combining all of my test like this, I can no longer determine which of my test actually failed. I'm not exactly sure what the best solution to that is (yet) and right now, there don't seem to be a lot of people using jsVerify or any other property based framework with Jasmine to get a lot of hints on how we might write tests that are easy to use.

Conclusion
----------

I encourage you to give jsVerify a try.  It really isn't that hard to pick up and hopefully, this short article will smooth over some of the problems you may have as you get started.  Despite the puzzles I mentioned above the advantages of using it over example based testing encourage me to see just how far I can push this framework.
