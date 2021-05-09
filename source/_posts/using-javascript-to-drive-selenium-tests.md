---
title: Using JavaScript to Drive Selenium Tests
tags:
  - bdd
  - javascript
  - selenium
url: 4012.html
id: 4012
categories:
  - Javascript
  - Selenium
date: 2016-08-16 06:30:00
---

I’ve written about [using Selenium to test web applications before](/tags/selenium/).  But all of those articles have assumed you are using C#.  I’ve realized that Selenium has multiple language bindings which allow me to use any language I want but C# just seemed easier at the time.  But, now I’m in an environment that doesn’t use .NET at all.  They use Java.  I know Java, but I choose not to use it and instead my focus at this shop is all JavaScript.  Which means, if I want to write Selenium tests to verify my work, I need to write my tests in JavaScript.  But Using JavaScript to Drive Selenium is, in my opinion what everyone should be doing.  At least everyone who is writing most of their web application using client side code.

<!-- more -->

Think about it, the primary language you wrote the client side with is JavaScript, and yet you are going to write your tests using C#, Java, PHP… when you could be writing the tests using JavaScript.  The same language you used to write the bulk of your application.  Further, when you write your tests using C# or Java, you’ll probably either write the tests using a unit testing framework (MSTest, NUnit, JUnit) or you’ll use some sort of BDD adapter like SpecFlow to coerce the unit testing framework into the behavioral pattern you need.

![image](/uploads/2016/08/image.png "image")  If, you use JavaScript, you can use Jasmine, which is already behavioral, to run your tests.

Another added advantage to the binding I’m going to recommend in this post, is that you get parallelization right out of the box.  You don’t need to write any funky code to [make your tests run in parallel](/running-selenium-in-parallel-with-any-net-unit-testing-tool/).  You get that for free.

Another advantage I see for large companies is that not every group is going to be using the same server side language.  Where I’m at there are several groups using .NET and several groups using Java.  But everyone is using JavaScript.  If we all use JavaScript to drive our Selenium tests, we can share the knowledge we learn with other groups.

So, let’s get started.

For the purposes of this article, I’m going to assume you’ve already [setup your node environment using the instructions I provided a couple of weeks ago](/you-can-start-using-node-today/).  If you haven’t, you should do that now.  If you already have Node setup on your computer, but you are still using a version less that 6.3, I’m going to suggest you upgrade to the current version so that you can use some of the new ES2015 functionality like fat arrows.

Setup
-----

The tools we are going to use for testing using JavaScript, other than Node are:

*   [WebDriver.io](//webdriver.io/)
*   [WebDriver.io Spec Reporter](//webdriver.io/guide/reporters/spec.html)
*   [Jasmine](//jasmine.github.io/)

And while the documentation on the WebDriver.IO site is quite complete, you would need to wade through quite a bit of information you don’t need to get this all setup and going.  We are going to give you just enough to get started.  If you want more information, I invite you to visit the website I’ve linked to above.

For the purposes of this walk through, we are going to ignore the Selenium Grid part.  I’ve written about setting up Selenium Grid before.  Using the grid from WebDriver.io will just be a matter of changing some configurations.  Similarly, if you want to run the test from a cloud provider, the docs for that are on the website.

Here, we are just going to get setup with a standalone server.  This suits my typical use of Selenium is to write a test for one specific tests at a time.  Once I have it working, I disable the tests.  The only time I run all of my test is prior to deploying the code to DEV just to make sure I didn’t break anything along the way.  Besides, setting up a standalone environment gets you started quickly.

Before we really get started with the JavaScript stuff, make sure you have Java installed on your computer.  You’ll need that to run the Selenium stuff.

So, to start out, install WebDriver.io First create a project directory for your new test project.  You can, of course, name it whatever you want.  Just make sure you make that directory your current working directory (CD into it.) Once you’re in the project directory, create your package.json file by typing in npm init Which will ask you several questions about your project.  Answer the questions and/or accept the defaults.

Then install WebDriver.io by using the NPM command `npm install webdriverio --save-dev` This should place wdio in your ./node_modules/.bin directory.  Run `./node_modules/.bin/wdio --help` To verify that it is working.  If you are running on a Windows computer, you’ll want to make those forward slashes back slashes.

Now that it is working, we are going to use it to configure a test runner file.  Type in ./node_module/.bin/wdio config One of the questions it is going to ask is which framework you want to use.  Pick ‘Jasmine’ Then it will ask if it should install the framework for you, pick ‘Yes’ Then it will ask where your tests are located.  Type in the correct location.

Then it will ask which adapter you want to use.  Pick ‘spec’ and have it install that library for you.

For Service, select ‘selenium-standalone’ and let it install that for you as well.  (Noticing a theme here?) For base URL, pick [https://www.google.com](//www.google.com) because we are going to write out demo tests against google.

Now sit back and let the wdio config install the missing parts it needs for your tests.

By default, this setup makes FireFox the test browser.  You can change that using the instructions located here: [http://webdriver.io/guide/testrunner/configurationfile.html](//webdriver.io/guide/testrunner/configurationfile.html "http://webdriver.io/guide/testrunner/configurationfile.html") I know that looks like a lot of setup, but it really is pretty easy.  One NPM command to install webdriver.io and another wdio command to get the config file setup and the rest of the dependencies installed.  Pretty sweet.

Now, let’s write our first test.

Writing Tests
-------------

We are going to put our test in the directory we specified in our config file.  In my case, I put my tests under /tests/**/*.js Just to make sure everything is working correctly, we are going to write a pretty simple first test.

``` typescript
"use strict";
describe('First test',()=>{
    beforeEach(()=>{
        browser.url('https://www.google.com');
    });
    it('should display "Google" in the title',()=>{
        expect(browser.getTitle()).toBe('Google');
    });

});
```

There are some bits that might look new here.

First, because we are running this in node, only the file has scope so we can place ‘use strict’; at the top.

Second, we are making use of the fat arrow functions.  For our purposes today, ()=>{} is the same as function(){}.  Just a bit easier to write.

Third, we are using a browser variable that hasn’t been defined anywhere.  At least not that we can see.  This is a design decision the WebDriverIO team made that I don’t necessarily agree with.  We should require() it into our module when we need it.  But it is what it is.

You can checkout the API for the browser object here: [http://webdriver.io/api.html](//webdriver.io/api.html "http://webdriver.io/api.html") So, our test is loading the default URL and checking the contents of the title tag.

Now we run our test with wdio `./node_modules/.bin/wdio wdio.conf.js` Our test should pass.

Page Objects
------------

So that’s the basics.

If you are already familiar with writing Selenium testing, that’s the guts of what you need to know to get going.

If you aren’t, or you’ve never heard of Page Objects, that is what I plan to cover next.

Generally, when we write our selenium test, we want to use a Page Object.  That is, we want to hide all of our Selenium specific stuff under an object so we can write our test without having to worry about the location of the elements on the screen changing.

You see, here’s the problem you are going to run into.  You are going to write lots of tests.  At least I hope you do.  And then someday, you are going to want to change an ID of an element, or otherwise change how you find it.  If you write all of the tests so that they code directly to the Selenium code, you’ll need to find every place you were looking for that element and replace it with the new lookup.  Not very DRY.

But, if you create a page object, you’ll put your code there and call it from your tests.

So for an example.  Let’s create a GooglePage definition.  Once again, because we are using Node 6.3 (or above) we can use the class keyword.  The browser variable is still available to us because it is global.

So, a pretty simple page object for Google search might look like this:

``` typescript
class GooglePage{
    static load(){
        browser.url('https://www.google.com');
    }
    static get title(){
        return browser.getTitle();
    }
    static get searchInput(){
        return browser.element('input[name="q"]');
    }
    static get searchButton(){
        return browser.element('input[name="btnK"]');
    }
    static search(value){
        GooglePage.searchInput.setValue('Node.js');
        browser.pause(1000);
    }
};

module.exports = GooglePage;
```

And then to access it from our test, we modify the test file above to look like this:

``` typescript
"use strict";
var GooglePage = require('../pages/GooglePage');
describe('First test',()=>{
    beforeEach(()=>{
        GooglePage.load();
    });
    it('should display "Google" in the title',()=>{
        expect(GooglePage.title).toBe('Google');
    });
    describe('and I search for "Node.JS"',()=>{
        beforeEach(()=>{
            GooglePage.search('Node.JS');
        });
        it('should display "Node.JS" in the title',()=>{
            expect(GooglePage.title).toBe('node.js - Google Search');
        });
    })

});
```

This is a pretty simple example for demonstration purposes.  If you’ve never done any kind of Selenium testing before, I recommend you dig a little deeper than where I’ve gone here

Project
-------

The completed project is available on GitHub here [https://github.com/DaveMBush/WebDriverIO-Sample](//github.com/DaveMBush/WebDriverIO-Sample "https://github.com/DaveMBush/WebDriverIO-Sample")    

Photo credit: [smjbk](//www.flickr.com/photos/smjb/8107539280/) via [Visualhunt.com](//visualhunt.com) / [CC BY](//creativecommons.org/licenses/by/2.0/)