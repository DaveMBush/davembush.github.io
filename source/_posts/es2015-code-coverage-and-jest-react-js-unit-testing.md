---
title: ES2015 Code Coverage and Jest (React JS Unit Testing)
tags:
  - babel
  - es2015
  - es6
  - jest
  - react.js
  - unit test
url: 3897.html
id: 3897
categories:
  - Javascript
date: 2016-05-05 07:30:00
---

As I’ve [mentioned before](/react-js-and-associated-bits/), I’m in the middle of putting together a React reference app and I’m doing it using [Test Driven Development](/test-driven-learning-an-experiment/).  The problem is, the standard tools for implementing ES2015 code coverage with Jest make it hard to see at a glance if you have 100% code coverage or not because of some issues with the way Jest tells Babel to do the transformations by default, the way Babel transforms the code and implements the auxiliaryCommentBefore option and the way that Istanbul parses the ignore next comments. I’ve been working on solving this problem for the last month and a half off and on.  I’ve even posted a question about this on [Stack Overflow](//stackoverflow.com/questions/35986316/reactjs-0-9-code-coverage-with-es2015-class-keyword), so I’m pretty sure no one else has a solution for this yet.  I’m not going to say my solution is the best way to solve this problem, but it is a solution, which is better than what we have so far. ![ES2015 Code Coverage and Jest](/uploads/2016/04/image-5.png "ES2015 Code Coverage and Jest") 

Diagnostics
-----------

By default, when Babel transforms your code, it inserts additional functions into the code that it can call to replace the code you wrote that does not yet conform to the syntax you’ve used.  This code gets inserted at the top of the file and shows up in your code coverage reports as several conditions that didn’t get fired.  Yes, it inserts code it never uses because the functions have to work under a variety of scenarios. For those who are interested in how I figured this out.  The transform results are located in node\_modules/jest-cli/.haste\_cache.

ES2015 Code Coverage Fix One
----------------------------

OK, so the standard recommended fix for something like this is to place

/\* istanbul ignore next */

Prior to those functions.  And it just so happens that both Jest and Babel provide a mechanism for adding this comment by using the auxiliaryCommentBefore option. Only there are two problems with this.

### Problem One

If you just set the property like this:

auxiliaryCommentBefore: 'istanbul ignore next'

Your code will get transformed so that any functions added by Babel will end up looking like this:

/\*istanbul ignore next\*/function babelFunctionHere()...

But in order for Istanbul to pickup this comment, the code needs to look like this:

/\* istanbul ignore next */ function babelFunctionHere()...

While getting the spaces on either side of ‘istanbul ignore next’ is a simple matter, we have no real control over the space that is necessary between the comment marker and the function keyword.

### Problem Two

The second problem with this “fix” is that even if modify the Babel code so that the comment gets inserted correctly, it doesn’t get inserted before EVERY function that Babel inserts.  If it inserts a group of functions, which it does regularly in my code, it only inserts the comment before the first function.

ES2015 Code Coverage Fix Two
----------------------------

What if we didn’t insert the functions in our code?  Well, it just so happens that we can do that relatively easily. There is a plug-in for Babel called ‘[transform-runtime](//www.npmjs.com/package/babel-plugin-transform-runtime)’.  What this plug-in does is that it requires in the functions rather that pasting them into your code.  This way, the functions don’t exist in your code so Istanbul never sees the function block.  Pretty cool. You can add this to either your .babelrc file or the Babel section of your package.json file by adding a “plugins” section

"plugins": \["transform-runtime"\]

along with the “presets” section you should already have.

Remaining Issue
---------------

While using transform-runtime takes care of most of the issues, there are two functions that still don’t get covered.  In fact, when you look at the transform-runtime code, you find that they are explicitly excluded and if you include them, your code won’t transpile at all. The good news is, it is only two functions and they both show up as

function _interop...

If we can get a hold of the code as it is being transformed, we should be able to do a search and replace to get the correct ‘istanbul ignore next’ string in place prior to the functions. Well, it just so happens that Jest has the ability to do exactly that.

ES2015 Code Coverage Final Fix
------------------------------

I’m assuming you’ve already installed [babel-jest](//www.npmjs.com/package/babel-jest), but just in case, if you have not, install it now.  Install it using –save-dev because we are going to want to be able to modify the code.

### Quick fix:

The proper way to fix this would be to write your own version of babel-jest.  But we are going for a quick fix.  Maybe we can get Facebook to implement the changes from this post.  Meanwhile, here is what you want to do. Locate the src/index.js file in the node_modules/babel-jest directory.  At the time of this writing, the current version looks like this:

/**
 \* Copyright (c) 2014-present, Facebook, Inc. All rights reserved.
 *
 \* This source code is licensed under the BSD-style license found in the
 \* LICENSE file in the root directory of this source tree. An additional grant
 \* of patent rights can be found in the PATENTS file in the same directory.
 */

'use strict';

const babel = require('babel-core');
const jestPreset = require('babel-preset-jest');

module.exports = {
  process(src, filename) {
    if (babel.util.canCompile(filename)) {
      return babel.transform(src, {
        auxiliaryCommentBefore: ' istanbul ignore next ',
        filename,
        presets: \[jestPreset\],
        retainLines: true,
      }).code;
    }
    return src;
  },
};

The first change that you want to make here is to comment out the auxiliaryCommentBefore line.  We no longer need that.

'use strict';

const babel = require('babel-core');
const jestPreset = require('babel-preset-jest');

module.exports = {
  process(src, filename) {
    if (babel.util.canCompile(filename)) {
      return babel.transform(src, {
//        auxiliaryCommentBefore: ' istanbul ignore next ',
        filename,
        presets: \[jestPreset\],
        retainLines: true,
      }).code;
    }
    return src;
  },
};

You will notice that what gets returned is the resulting transform of the code.  We want to execute a search and replace on the transformed code.  So, instead of

     return babel.transform(src, {
        auxiliaryCommentBefore: ' istanbul ignore next ',
        filename,
        presets: \[jestPreset\],
        retainLines: true,
      }).code;

What we want want to do is:

      return babel.transform(src, {
        //auxiliaryCommentBefore: ' istanbul ignore next ',
        filename,
        presets: \[jestPreset\],
        retainLines: true,
      }).code
          .replace(/function\\s_interop/g,
              ' /\* istanbul ignore next */ function _interop');

ES2015 Code Coverage With Jest - Summary
----------------------------------------

1.  Download and install babel-plugin-transform-runtime.
2.  Add “plugins”: \[“transform-runtime”\] to either .babelrc or the babel section of your package.json file
3.  Download and install babel-jest
4.  Modify babel-jest/src/index.js as indicated above.

Better Solution?
----------------

I would prefer to have a babel plugin that did the search and replace.  If you know of one, let me know in the comments.
