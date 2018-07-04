---
title: Angular Ionic and Angular CLI
tags:
  - angular
  - angular-cli
  - ionic
url: 4501.html
id: 4501
comments: false
categories:
  - Angular 2
date: 2017-11-14 06:30:12
---

As you might have noticed from last week's post, I've shifted my focus from pure Angular to learning Angular Ionic.  And while last week's post focused more on just [getting Ionic setup on a Windows environment](/angular-ionic-putty-ssh-authorized_keys-format), this post will focus more on integrating Ionic and Angular CLI to work together. If you are familiar with Ionic, you should already know that it provides its own CLI that allows you to scaffold out a new application using a basic template.  This CLI is also used to help register the project with the Ionic Dashboard and scaffold out a limited number of file types if you use Ionic 3.  However, there are several problems I have with using the Ionic CLI.  First, and probably most important to me, is there is no test scaffolding!  Second, it neither follows the standard naming convention for files nor does it comply with the Angular Style Guide when it comes to directory structure. My first attempt at correcting the problem was to try to add Ionic to an existing Angular CLI project.  I almost had that working, but I got stuck trying to get the SCSS implementation working.  I finally gave up once I realized that Ionic seems to load files on demand, including SCSS files and templates.  I might come back to this once I've gained more experience with Ionic and have a better idea of how it works under the hood. So, my second thought was to just add the Angular CLI to an existing Ionic CLI project.  It turns out this was much easier to get working.  This allows me to use the standard `ng` commands to scaffold out my components, services, interfaces, etc... and because I'm using the Angular CLI scaffolding, the tests also get scaffold out for me. <figure>![](/uploads/2017/11/2017-11-14.jpg "Angular Ionic and Angular CLI")<figcaption>Photo credit: [Internet Archive Book Images](//www.flickr.com/photos/internetarchivebookimages/14762635481/) via [VisualHunt.com](//visualhunt.com/re/09daa4) / [ No known copyright restrictions](//flickr.com/commons/usage/)</figcaption></figure>

<!-- more -->  

For the purposes of this post, I'm going to assume you've already created a project using the Ionic CLI.  What follows are the steps you need to take to add in the Angular CLI.

Add Angular CLI
---------------

All of the additions to the package.json file are in devDependencies.  The version numbers I've included are for the version at the time of this writing. You'll want to modify for newer versions as they are released. First, of course, add the angular cli

``` json
 "@angular/cli": "1.4.9" 
```

This will give you the ability to use the `ng` commands to scaffold out files.  To allow the tests to run, you'll also want to add the following:

``` json
"@types/jasmine": "~2.5.53",
"@types/jasminewd2": "~2.0.2",
"@types/node": "~6.0.60",
"jasmine-core": "~2.6.2",
"jasmine-spec-reporter": "~4.1.0",
"karma": "~1.7.0",
"karma-chrome-launcher": "~2.1.1",
"karma-cli": "~1.0.1",
"karma-coverage-istanbul-reporter": "^1.2.1",
"karma-jasmine": "~1.1.0",
"karma-jasmine-html-reporter": "^0.2.2",
"protractor": "~5.1.2",
"ts-node": "~3.2.0",
```

I also use tslint quite extensively in my projects, so I also add in the following:

``` json
"codelyzer": "~3.2.0",
"tslint": "~5.7.0",
"tslint-immutable": "^4.4.0",
```

.angular-cli.json
-----------------

The main file that makes the Angular CLI recognize the project as an Angular CLI project is the `.angular-cli.json` file.  The easiest thing to do is to take an existing file from another project and copy it into your Ionic project.  This file belongs in the root of your project. Once you've done that, there is one small change you need to make.  The `main.ts` file that is the entry point of the application lives under the `app` directory in an Ionic project rather than directly under `src` like it does with an Angular CLI project.  It would be tempting to change the location of the file, but the Ionic build process is looking for it under the `app` directory.  An easier fix is to change the location in our `.angular-cli.json` file so that our tests can find it.  It is the only place that needs to know of the new location. Change .angular-cli.json so main points to the right location:

``` json
{
  "$schema": "./node_modules/@angular/cli/lib/config/schema.json",
  "project": {
      "name": "ionic-with-angular-cli"
  },
  "apps": [
  {
      "root": "src",
      "outDir": "dist",
      "assets": [
          "assets",
          "favicon.ico"
      ],
      "index": "index.html",
      **"main": "app/main.ts",**
      "polyfills": "polyfills.ts",
      "test": "test.ts",
 ...
}
```

karma.conf.js
-------------

To run your tests, you'll need a `karma.conf.js` file in your root directory.  Again, the easiest thing to do is to copy this file over from a project you created with the Angular CLI.

tsconfig.json
-------------

You should already have a `tsconfig.json` file in your project root directory.  If you open it, you'll see it has an exclude section that is excluding `node_modules`.  You'll need to tell it to exclude `test.ts` and `*.spec.ts` files so it doesn't pick up the test files during a normal compile cycle.

``` json
"exclude": [
  "node_modules",
  "test.ts",
  "**/*.spec.ts"
],
```

polyfills.ts
------------

The tests use the `polyfills.ts` file, so you'll need to add one that was originally created using the Angular CLI.  This belongs in your `src` directory.

test.ts
-------

Copy a `test.ts` file into your `src` directory from a project that you created with the `Angular CLI`.  If you don't put this file in, you won't be able to run any tests.

tsconfig.spec.json
------------------

The tests use `tsconfig.spec.json` located in the `src` directory.  The problem is, a standard `tsconfig.spec.json` file inherits from the `tsconfig.json` file we just modified above which doesn't look at all like the `tsconfig.json` file in an Angular CLI project.  I found the easiest thing to do was to create a new copy of `tsconfig.spec.json` that is a merge of the two files in an Angular CLI project.  This worked at the time of this writing, if you run into trouble, you may need to merge the two files you started with on your own.  My result looks like this:

``` json
{
  "compileOnSave": false,
  "compilerOptions": {
    "outDir": "../out-tsc/spec",
    "sourceMap": true,
    "declaration": false,
    "moduleResolution": "node",
    "emitDecoratorMetadata": true,
    "experimentalDecorators": true,
    "target": "es5",
    "typeRoots": [
      "node_modules/@types"
    ],
    "lib": [
      "es2017",
      "dom"
    ],
    "baseUrl": "./",
    "module": "commonjs",
    "types": [
      "jasmine",
      "node"
    ]
  },
  "files": [
    "test.ts"
  ],
  "include": [
    "**/*.spec.ts",
    "**/*.d.ts"
  ]
}
```

typings.d.ts
------------

I'm not sure this file was needed, but it was another file I found in an Angular CLI project that wasn't in my Ionic CLI project, so I added it.  It goes in your `src` directory and is just a copy of the file that is in your Angular CLI project.

pages
-----

You'll notice that your `pages` directory is at the same level as your `src` directory which doesn't comply with the Angular CLI.  I moved mine so that it is under the `src` directory just to keep things consistent.  You'll need to fix up imports once you move it of course.

Regenerate Pages
----------------

Because the pages that got generated don't conform to the style guide and don't have any tests, I just regenerated mine and copied the relevant code into the newly generated files.

Tests
-----

At this point, you should have a project that looks a lot more like an Angular CLI project.  You'll still use the Ionic command to do your regular build and development cycle and the ng commands for testing and generating new files.  Running tests works the same as you should already be used to from using Angular.  The one thing you will notice is that you'll see a warning in your test: `Critical dependency: the request of a dependency is an expression` This is a known "error" that neither the Angular CLI group or the Ionic group seem interested in addressing. At this point, I haven't tried any E2E tests using this setup so I don't know if there are any additional tweaks that need to be made. You can find the code for this at this branch of a project I'm working on. [https://github.com/DaveMBush/ionic-crud/tree/Add-Angular-CLI](//github.com/DaveMBush/ionic-crud/tree/Add-Angular-CLI)
