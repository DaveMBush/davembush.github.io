---
title: Unit Testing an Angular 2 CLI Project
tags:
  - angular 2
  - javascript
  - typescript
  - unit test
url: 4097.html
id: 4097
categories:
  - Angular 2
date: 2016-11-22 19:30:00
---

This week we want to continue our series about Angular 2 by looking at the Unit Testing capabilities that Angular 2 provides for us.  What we want to cover today is:

*   Tweaking Karma to avoid using the Browser Window
*   Code Coverage
*   Tips to testing components

This article was written using Angular CLI version 1.0.0-beta.20-4  (Tip, if you are upgrading on windows, `rm –rf node_modules dist temp` just means to delete the three directories.  You can do that part manually, or install bash for Windows and run the command in bash.) <figure>![](/uploads/2016/11/image-3.png "Unit Testing an Angular 2 CLI Project")<figcaption>Photo credit: [jimmiehomeschoolmom](//www.flickr.com/photos/jimmiehomeschoolmom/4427775569/) via [VisualHunt.com](//visualhunt.com) / [CC BY-NC-SA](//creativecommons.org/licenses/by-nc-sa/2.0/)</figcaption></figure>

<!-- more --> 

Tweaking Karma
--------------

Open up the project we’ve been working on.

*   [Getting Started With Angular 2](/getting-started-angular-2/)
*   [Adding CSS and JavaScript to an Angular CLI Project](/adding-css-and-javascript-to-an-angular-2-cli-project/)

Drop into command line mode and run `ng test` The first thing you will notice is that this brings up the Chrome browser to run your test.  I don’t know about you, but I really dislike having a browser window up.  I have enough windows running on my screen as it is.  This is the first thing we need to fix.  To do this we are going to install PhantomJS. `npm install --save-dev phantomjs-prebuilt` Then, we need to tell karma to use PhantomJS.  This is a two step process.  First, we install the karma phantomjs runner `npm install --save-dev karma-phantomjs-launcher` Next, we modify the karma.conf.js file Change `require('karma-chrome-launcher'),` and `browsers: ['Chrome'],` To `require('karma-phantomjs-launcher'),` and `browsers: ['PhantomJS'],` Last, since we are not using the browser, we will need a better reporting mechanism. To do this we will install spec reporter. `npm install --save-dev karma-spec-reporter` and we replace this line in karma.conf.js

reporters: config.angularCli && config.angularCli.codeCoverage
          ? \['progress', 'karma-remap-istanbul'\]
          : \['progress'\],

with

reporters: config.angularCli && config.angularCli.codeCoverage
          ? \['spec', 'karma-remap-istanbul'\]
          : \['spec'\],

And we add a require line at the top of the file with the other requires `require('karma-spec-reporter'),` Now, when we run `ng test` We get a nice text report in our terminal windows instead of the browser popping up.

Code Coverage
-------------

To get a code coverage report for our test use the command ng test –cc The code coverage files will end up in a directory named ‘coverage’ hanging off the root of your project.  You can view the coverage/index.html file to see how well your files are covered.

Testing Components
------------------

For the purposes of this article, I’m going to assume you have some familiarity with creating Jasmine tests.  If you don’t the documentation for Jasmine is pretty easy to understand. But what I do want to cover here is how we test components. If you open up the `app.component.spec.ts` file, you’ll notice a `TestBed` class that gets used quite a bit.  Since your test isn’t going to test modules because they only package our other code together, you need some way of faking that out so you can test the components.  To do that, you use `TestBed.configureTestingModule(` { declarations: \[ AppComponent \] }); Which just sets up the declarations you need to reference the component you need to test.  You can put anything in this block that you would normally put in your module definition. Another `TestBed` method you’ll see is `TestBed.createComponent()`, which you probably have guessed, creates an instance of the module so you can test it.  The object it creates has a `debugElement` property hanging off of it.  There are two properties that hang off this object that you’ll make use of a lot.  `componentInstance` is the actual instance of the component that you created.  Any properties and methods that your component has will be available off of `componentInstance`. The other object that will be available is `nativeElement`.  This is the DOM element that the component renders to and you can use `querySelector(cssSelectorGoesHere)` to select the first element matches the selector or `querySelectorAll(cssSelectorGoesHere)` to retrieve an array of elements that match. Of course, a test isn’t any good if you don’t make changes to the component and test for them.  And for that we have `detectChanges()`.  You’ll see that being used in the third test.  You’ll want to use that before you `expect()` anything.

Ready, Set, …
-------------

Now that we have some way of testing our code, we can actually begin to write so.  Don’t forget to subscribe to the email I sent out so you don’t miss the next article in this series. Code so far is located at [https://github.com/DaveMBush/GettingStartedWithAngular2/tree/Step2](//github.com/DaveMBush/GettingStartedWithAngular2/tree/Step2)
