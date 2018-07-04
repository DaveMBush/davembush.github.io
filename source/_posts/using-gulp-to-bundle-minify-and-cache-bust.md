---
title: 'Using Gulp to Bundle, Minify, and Cache-bust'
tags:
  - bundling
  - gulp
  - minification
  - node.js
url: 3361.html
id: 3361
categories:
  - Javascript
  - Node.js
date: 2016-01-28 08:30:00
---

Last week I discussed [how to setup Node.js and Gulp in Visual Studio 2015](/using-node-js-and-gulp-with-an-existing-web-application-in-visual-studio-2015/).  During that discussion, I mentioned that I’m using gulp to bundle, minify and cache-bust my HTML, CSS, and JavaScript files. This week, my intent is to walk you through exactly how I do that. So, if you don’t already have Node.js and Gulp installed, you may want to go back and read the article I wrote last week. Since most of the people who read this blog are ASP.NET developers, there may be a few .NET specific tips along the way.  But the Gulp file I am going to walk you through is technology agnostic.  So if you are using some other technology, you’ll still benefit from this article. ![image](/uploads/2016/01/image-5.png "image")

Bundling
--------

The first thing we want to do is that we want to combine all of our CSS files and JavaScript files into one file for CSS and one file for JavaScript.  There are several ways that you might do this, but what I wanted was some way that would allow me to work unbundled during development and bundled when I released the code.  Since I’m working with a single page application (SPA), this was a simple matter of configuring my default file to be index.debug.html for development and index.release.html and using web.config transforms to specify which should be used in which environment.  If you are working in some other environment, I’m sure you have some kind of similar way of specifying the default file based on an environment setting. So all of my development work will be done in index.debug.html. Like I said, there are many ways of bundling JavaScript and CSS code, but the way I’ve found that seems to have the least amount of work involved is by using the [gulp-useref module](//www.npmjs.com/package/gulp-useref).  This module lets us put tokens in our html file that specify which files we want to compress and what the resulting file name should be. Your HTML file would look something like this:

<html>
<head>
  <!\-\- build:css css/combined.css -->
    <link href="css/one.css" rel="stylesheet">
    <link href="css/two.css" rel="stylesheet">
  <!\-\- endbuild -->
</head>
<body>
<!\-\- other normal content goes here -->
  <!\-\- build:js scripts/combined.js -->
    <script type="text/javascript" 
      src="scripts/one.js"></script> 
    <script type="text/javascript" 
      src="scripts/two.js"></script> 
  <!\-\- endbuild -->
</body>
</html>

useref sees the build token and creates a css file named combined.css and a JavaScript file named combined.js and changes the output html so that it looks like this:

<html>
<head>
  <link rel="stylesheet" href="css/combined.css"/>
</head>
<body>
  <script src="scripts/combined.js"></script> 
</body>
</html>

To use this feature in your Gulp file, install it using NPM

npm install gulp-useref --save-dev

Make sure that you run NPM in the directory your gulpfile.js file is in. In your gulp file, you’ll add the following code:

var gulp = require('gulp');
var useref = require('gulp-useref');

gulp.task('useRef', \[\],function() {
    return gulp.src('index.debug.html')
        .pipe(useref())
        .pipe(gulp.dest('index'));
});

The end result is that there will be a new index.debug.html file in a sub directory named index along with the new css/combined.css and scripts/combined.js file.

Conditional Processing
----------------------

One of the many things I like about Gulp is that it is stream based.  That is, I don’t have to write files to a directory unless or until I want to.  Unlike Grunt (the other popular file processing Node.js based tool) where everything is entirely file based. However, because I have three different types of files coming out of the previous process, and I want to compress each of the files, I’ll need some way to conditionally process the files coming out of useref. For this, we need [gulp-if](//github.com/robrich/gulp-if). You can install gulp-if using NPM using the following command:

npm install gulp-if --save-dev

The basic using of gulp-if looks like this:

var gulpif = require('gulp-if');

gulpif(/\* file condition here */, /\* next stream process goes here*/);

Minify JavaScript
-----------------

The next thing we want to do is that we want to minify the resulting combined JavaScript file.  There are several that you could use.  The one I settled on is [gulp-uglify](//www.npmjs.com/package/gulp-uglify). Install gulp-uglify using

npm install gulp-uglify --save-dev

So, combining this with gulp-if, the usage would look like this:

var gulpif = require('gulp-if');
var uglify = require('gulp-uglify');

... other code here

.pipe(gulpif('*.js', uglify()));

This is just the general gist.  We’ll put all of this together in a few more paragraphs.

Minify CSS
----------

To minify CSS, I decided to use [gulp-cssnano](//www.npmjs.com/package/gulp-cssnano) Install gulp-cssnano using:

npm install gulp-cssnano --save-dev

And use it in our code like this:

var cssnano = require('gulp-cssnano');
var gulpif = require('gulp-if');

... code here ...

.pipe(gulpif('*.css', uglify()));

Map Files
---------

Once we have all of our files minified, we’ll want some way of being able to see the original source code even though you have a minified file that the site is using.  I’m not going to go into a lot of detail about what a map file is or what it does here.  But if you have a problem on a production server, you are going to want to at least have map files available so you can track the issue down. To create a map file, you’ll want to use [gulp-sourcemaps](//www.npmjs.com/package/gulp-sourcemaps). Install using

npm install gulp-sourcemaps --save-dev

I’ll show you how this all plugs in soon.

One More Package
----------------

Yes, believe it or not, there is one more package we need yet to make this all work. You see, to get the minify stuff to work with useref we need to install the [lazypipe](//www.npmjs.com/package/lazypipe) module. Which you can install using:

npm install lazypipe --save-dev

Putting it All Together
-----------------------

And so now, finally, we can put this all together in one big script.

var gulp = require('gulp');
var useref = require('gulp-useref');
var uglify = require('gulp-uglify');
var cssnano = require('gulp-cssnano');
var lazypipe = require('lazypipe');
var sourcemaps = require('gulp-sourcemaps');
var gulpif = require('gulp-if');

// compressTasks is a sub process used by useRef (below) 
// that compresses (takes out white space etc) the 
// javascript and css files
var compressTasks = lazypipe()
    .pipe(sourcemaps.init, { loadMaps: true })
    .pipe(function () { return gulpif('*.js', uglify()); })
    .pipe(function() {
        return gulpif('*.css', cssnano({
                zindex: false }));
    });

// useRef looks at markers in index.debug.html and 
// combines all of the files into one file.  once the 
// files are combined the compressTasks process 
// is called and then the files are all written out to 
// the index directory.
gulp.task('useRef', \[\],function() {
    return gulp.src('index.debug.html')
        .pipe(useref({}, 
            lazypipe()
            .pipe(compressTasks)
            
            ))
        .pipe(sourcemaps.write('.'))
        .pipe(gulp.dest('index'));
});

Compressing The HTML file
-------------------------

So, as the comments say and as I’ve mentioned before, this places everything in the index directory.  What we want to do next is to compress the HTML file and move the index.debug.html back up to the root directory with the name index.release.html and place the css file and the javascript file in the appropriate directories hanging off the root. To compress the HTML file, you can use the NPM module [gulp-htmlmin](//www.npmjs.com/package/gulp-htmlmin), which you can install using

npm install gulp-htmlmin --save-dev

And the code

var htmlmin = require('gulp-htmlmin');

// minIndex takes all of the whitespace out of the 
// main index file
gulp.task('minIndex', \['useRef'\], function() {
    return gulp.src('index/index.debug.html')
        .pipe(htmlmin({ collapseWhitespace: true,
             removeComments: true }))
        .pipe(gulp.dest('index'));
});

Rename the Index File
---------------------

For renaming the file, we use [gulp-rename](//www.npmjs.com/package/gulp-rename)

npm install gulp-rename --save-dev

And the following code:

var gulpRename = require('gulp-rename');

// renameIndex renames the index file and puts it
// in the root directory
gulp.task('renameIndex', \['minIndex'\], function () {
    return gulp.src('index/index.debug.html')
        .pipe(gulpRename('index/index.release.html'))
        .pipe(gulp.dest('.'));
});

This renames the file and puts it in the root directory.

Moving Files
------------

Next we need to get the files that are in our index directory back out to the directories the belong in.  To do this, we use the built-in node command gulp.dest().

gulp.task('copyJs', \['useRef'\], function () {
    // copy the js and map files generated from useref to 
    // the real app directory
    return gulp.src('index/app/*.*')
        .pipe(gulp.dest('app'));
});

gulp.task('copyCss', \['useRef'\], function () {
    // copy the css and map files generated from useref to 
    // the real css directory
    return gulp.src('index/css/*.*')
        .pipe(gulp.dest('css'));
});

Cache-busting
-------------

One of the age old problems of using CSS and JavaScript pages on our site is that when we put new versions up, we have no way of telling the browser that we just put a new file up unless we change the file name.  The trick is to make the file look like a new file.  This is typically done by putting a query parameter at the end. Once again, there are several solutions to this problem available for Gulp, but the one I like the best reads the file and generates a hash string for it and appends that as the query string.  This make the file look unique but only causes the browser to download the file if it really is different. To implement cache-busting, you’ll want to install [gulp-cache-bust](//www.npmjs.com/package/gulp-cache-bust)

npm install gulp-cache-bust --save-dev

And the final bit of code to make all of this work:

var cacheBuster = require('gulp-cache-bust');

// cacheBuster looks at the css and js files and appends a hash to the
// request to cause the file to get reloaded when the file changes.
gulp.task('cacheBuster', \['copyCss', 'copyJs', 'renameIndex'\], function () {
    return gulp.src('index/index.release.html')
        .pipe(cacheBuster())
        .pipe(gulp.dest('.'));
});

Enhancements
------------

If you wanted to go to the trouble, you could create this as one great big script that never actually put the files in the index directory.  But I have found having the files written out to the intermediate directory to be valuable for debugging purposes.

The Final Code
--------------

You can get [the complete Gulp script from GitHub](//github.com/DaveMBush/BundleMinifyAndCacheBustWithGulp).
