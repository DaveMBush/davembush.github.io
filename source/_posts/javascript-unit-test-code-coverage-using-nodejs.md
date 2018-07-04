---
title: JavaScript Unit Test Code Coverage Using NodeJS
tags:
  - code coverage
  - istanbul
  - jasmine
  - javascript
url: 3369.html
id: 3369
categories:
  - Javascript
date: 2016-02-04 08:30:00
---

A couple of weeks ago, I showed how to get [Node.JS and Gulp working with Visual Studio 2015](/using-node-js-and-gulp-with-an-existing-web-application-in-visual-studio-2015/).  Last week I showed you [how to bundle, minify, and cache-bust using Gulp](/using-gulp-to-bundle-minify-and-cache-bust/).  This week, we are going to use Node.js to provide JavaScript Unit Test Code Coverage. The main tools we will be using to pull this off are Karma and Istanbul.  The test we write will be using Jasmine. If you don’t use Visual Studio, you should still be able to adapt these instructions to your own environment.  I’ve found getting Istanbul setup kind of tricky at times.  Since everything I’m going to show you here is pure Node.JS, you can ignore the Visual Studio parts. Let’s get started. ![JavaScript Unit Test Code Coverage Using NodeJS](/uploads/2016/01/image-6.png "JavaScript Unit Test Code Coverage Using NodeJS") 

Assumptions
-----------

I’m going to assume that you’ve already got a project setup.  For the purposes of this discussion, we are going to assume that the files we want to test are in the /app directory and that our test are in the /jsTest directory. If you are using Visual Studio, one of the first questions you might have is, “if I put my tests in the same project as the app I am testing, won’t those test get deployed with the application?”  And the answer to that question is, “Yes, if you use the defaults.” But we aren’t going to use the defaults.  What we are going to do is that we are going to make sure that any files we create that we don’t want to deploy to the web server have their build action set to “none”.  You can find this in the file’s property window. The other way you could solve this problem is that you could have a deploy script written in Gulp that specifies exactly what files should be deployed.

A Simple Demo File
------------------

Just so we have something to test, I’ve created a really simple demo JavaScript file that looks like this:

(function() {
    function demo() {
        var self = this;
        function add(a, b) {
            return a + b;
        }

        self.add = add;
    }

    window.demo = demo;
})();

Yes, just a simple add function.  But that is all we need today.

### Why the IIFE?

You may be wondering why I put an IIFE around such a simple demo. I’ve gotten so frustrated reading other people’s blog posts with demo code that confuses me because they have not used best practices for the framework they are using, that I’ve determined to always write my demo code as close to the way I would write production code as possible.  If I were writing a real system, I would place an IIFE around my JavaScript.  So, I’m doing it here.

And A Simple Test
-----------------

(function(describe,it,expect) {
  describe('/jsTests/app/demoTests', 
      function () {
        var demo;
        beforeEach(function() {
            demo = new window.demo();
        });
        it('demo should truthy', 
          function() {
            expect(demo).toBeTruthy();
        });
    });
})(window.describe, 
    window.it, window.expect);

### Why Pass In Global Variables?

By passing in the global variables, I can reference them as normal, but JSLint will no longer complain that I’m using an undefined variable.  Passing in the variables also places them in the local scope of the IIFE so that the test code doesn’t have to crawl all of the way up the scope chain to find the variables.  Finally, if I were to accidentally create a variable with the same name as a global variable, passing them into the IIFE will cause my development tools to warn me that I’ve overwritten a variable name.

Install Karma and Istanbul
--------------------------

The next thing you’ll want to do is to install Karma and Istanbul.  This is rather trivial because you can install both with one NPM command. `npm install karma karma-cli karma-coverage --save-dev`

Install Karma-Jasmine
---------------------

`npm install karma-jasmine --save-dev` If you are using some other test runner, you’ll need to install the appropriate karma package for it.

Install PhantomJS
-----------------

This is the final install you will need to make.  The truth of the matter is that you can use any browser to run your test.  But, normally, you’ll want to use a headless browser so that you can run the tests in your continuous integration server. When I am interested in seeing if my tests passed during development, I’ll run the tests in a regular browser using a regular HTML file.  Standard, old, jasmine tests.  When I want to see the code coverage, I’ll use PhantomJS. To use PhantomJS, go to the site and [download the zip file that contains the EXE](//phantomjs.org/download.html) and place it in your PATH environment variable.  Or, you can place it in a known location relative to your project and you can call it directly.  For this demo, we will place it in /jsTests/phantomjs. You will also need the phantom launcher.  There are several available, but the one I use just installs the launcher and assumes you already have it installed. `npm install karma-phantomjs-launcher-nonet –save-dev`

Karma.conf.js
-------------

The last step is to create a karma.conf.js file.  I typically put this in my jsTests directory because it is part of the test files. Your Karma.conf.js file should contain content that looks something like this:

module.exports = function (config) {
    var path = require('path');
    config.set({
        browsers: \['PhantomJS'\],
        phantomjsLauncher: {
            cmd: {
                win32: path.join(__dirname,
                     '/phantomjs/phantomjs.exe')
            }
        },
        // this tells Karma to start Jasmine
        frameworks: \['jasmine'\],
        files: \[
           '../app/**/*.js',
           '../jsTests/app/**/*.js'
        \],

        // coverage reporter generates the coverage 
        reporters: \['progress', 'coverage'\],

        preprocessors: {
            '../app/**/*.js': \['coverage'\]
        },

        // optionally, configure the reporter 
        coverageReporter: {
            type: 'html',
            dir: 'coverage/'
        }
    });
};

Run Your Tests
--------------

Unlike many of the demos for running Karma that are available.  We are going to run our tests in a slightly different way.  Using Gulp. Most people know of Gulp as a file processing tool.  But here we are going to just use its task running capability.

var gulp = require('gulp');
var Server = require('karma').Server;

gulp.task('test', function (done) {
    new Server({
        configFile: __dirname + '\\\jsTests\\\karma.conf.js',
        singleRun: true,
        browserNoActivityTimeout: 60000
    }, function () { done(); }).start();
});

This simple task will run Karma for you using the karma.conf.js file we just created in jsTests. If you want to have this run every time a file changes once you’ve kicked off this task, change singleRun to false.  As it is written, it only runs the tests on demand.

JavaScript Unit Test Code Coverage
----------------------------------

So, hopefully, you’ve got everything running correctly.  Let’s look at the results. The output for the code coverage should now be in /jsTests/coverage/PhantomJS* directory.  Load the index.html file in your browser. You should see a screen that looks something like this: ![image](/uploads/2016/01/image-7.png "image")   Click on ‘app/’ to see this: ![image](/uploads/2016/01/image-8.png "image") And finally, click on ‘Demo.js’ to see ![image](/uploads/2016/01/image-9.png "image")

The Shortcut
------------

Fortunately for you, [I’ve created a project on GitHub with all of this already done](//github.com/DaveMBush/CodeCoverageDemo).
