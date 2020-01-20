---
title: You Can Start Using Node Today
tags:
  - javascript
  - node
  - npm
  - nvm
url: 3999.html
id: 3999
categories:
  - Javascript
date: 2016-08-02 06:30:00
---

I was just getting started writing an article about using Node/JavaScript to drive my Selenium tests and as I was writing the “Prerequisite” section, I realized I have never written the basics about how to get setup with Node or even why you would want to.

As popular as Node is, I am still finding that many of the people I work with have no idea what it is or if they do, they only have a partial idea and can’t see how it would apply to the work they do on a daily basis.

So, let’s start with the fundamentals.

<figure>![](/uploads/2016/07/image-4.png "You Can Start Using Node Today")<figcaption>Photo credit: [stevendepolo](//www.flickr.com/photos/stevendepolo/5749192025/) via [VisualHunt.com](//visualhunt.com) / [CC BY](//creativecommons.org/licenses/by/2.0/)</figcaption></figure>

<!-- more -->

What is Node?
-------------

I think the best place to start with our introduction is by providing a comprehensive view of what Node is.  A rather simplistic description would be, “Node is the V8 JavaScript engine from the Chrome browser, wrapped in an executable, that lets you run JavaScript without a browser.” Now, with that simplistic definition, you might think, “Why would I want to do that?”  Maybe you are assuming that this implementation only lets you do the same stuff that you can do in the browser.  And this is where you would be wrong.

So, let’s start over.

Node allows you to write cross platform applications that will run directly on your computer using JavaScript as the language.

And now I hear everyone thinking, “Yeah, yeah, that was the promise of Java.” OK.  Maybe that’s true.  Time will tell.

What Can You Do with Node?
--------------------------

Several well-known applications use Node.  You may be surprised at how much node is already being used.  Here’s a short list:

*   Many Web Servers
*   Desktop Applications
*   Developer Build Processes

Does any of that surprise you? There is a [long list of companies](//github.com/nodejs/node/wiki/Projects,-Applications,-and-Companies-Using-Node) who are using Node for some part of their development process or infrastructure.  Some more notable implementations include:

*   VS Code by Microsoft – a code editor built using Node and Electron.
*   Slack Desktop App – uses Node and Electron

In short, you can do just about anything you can think of.

Installing Node
---------------

This falls under the category of “Things I wish I had known.” You can just go to the [Node site](//nodejs.org/en/) and download the version you want to use.  But at some point, you are going to want to use multiple versions of Node.  One version for one project and a different version for some other project.  For that, you are going to need a tool called ‘NVM’.  Life will be a lot easier if you just install NVM first and then install Node from there.

If you are running Windows, you’ll want to [grab NVM from here](//github.com/coreybutler/nvm-windows/releases).  Everyone else can [get NVM from here](//github.com/creationix/nvm). Once you have NVM installed, you should be able to run `nvm install _version.number.here_` or you can run `nvm install node` To install the latest version.

You can run this command for each version you want to have installed.

To see which versions are installed, you can run `nvm ls` And to use a specific version you can run `nvm use _version.number.here_`

Using the Node Package Manager
------------------------------

When you installed Node, you also installed the Node Package Manager(NPM).  For those of you who are coming from the Microsoft world, NPM is like NuGet.  It is how we install additional “Modules” (think libraries) into our Node environment.

The commands for NPM are pretty straight forward and most of the time, the documentation will tell you exactly what command to run to get it into your development environment.  But, it is helpful to know why you are running the various commands.

But, before you start installing Node packages into your development environment, you are going to want a package.json file.  The easiest way to create this in a form that NPM can use is to use the command `npm init` which will walk you through all of the questions you need to answer to create a proper package.json file.

The next command you are going to encounter is `npm install`.

But `npm install` has several switches that you’ll be using.  Each with a different purpose.

`npm install _packagename_`

This will install the most recent version of the package into your node_modules directory and record the dependency in your package.json file in the dependencies section.

If you want to be explicit about where you are saving the file you can use the `--save` flag.  It does the same thing as `npm install package`.

`npm install --save _packagename_`

Your other option for saving is

`--save-dev`.

This puts the dependency in the devDependencies section.

You might wonder why you would have two different dependency sections.

The reason for this is because you might have modules that you need simply to build the project. They aren’t needed when you deploy the project. So having the two different sections allows you to deploy without the extra set of modules.

Node JavaScript
---------------

I once had someone assert that even within the same versions of JavaScript, there are different versions of JavaScript.  His main point was that there are differences between JavaScript on the browser and JavaScript in Node.  I assert, they are the same version of JavaScript, but the API that is available, or required, is different based on the environment.

So, JavaScript on Node is syntactically no different from the JavaScript you write now.  However, Node does solve an age old problem we’ve had in client side code automatically. This problem is global scope pollution. If we write JavaScript that looks like this:

``` javascript
var a = 'abc';
```

Without wrapping the code in a function, the code will end up in global scope.

The fix for this is to wrap all of our files in immediately invoked function expressions (IIFEs).  I’ve written about using IIFEs before as a best practice for Angular programming.  In fact, it is a best practice for all client side JavaScript programming.

But, in Node it is completely unnecessary because Node puts each file in its own scope.  Putting something on the global scope is something you have to do intentionally.  This is good, but it does require us to write some extra code.

You see, the problem is, if all of the code we write is only local to the file we write it in, how are we going to be able to write code in a modular fashion?  We don’t want all of our code in one monolithic file.

Modules
-------

So, to handle this problem, Node implements two keywords/functions/variables (depends on how you think about it) We’ll just call them keywords for now.

The first is the requires() keyword.

``` javascript
var foo = require('fooScript');
```

This says go find the script “fooScript.js” and assign what was exported from it to our “foo” variable.  If the file you need to require has been installed with NPM, then all you need is the name of the module, like I did above.  But if you are requiring in a file from your own code, you’ll need to require using a path reference.  For you Windows people, this always works using forward slashes (/) not backslash (\\).

This probably leaves you asking the question, how does fooScript expose its content to the module requiring it? With code that looks like this:

``` javascript
module.exports = fooFunc;
```

This line normally appears at the end of a file.  In the case of the line above, assume that fooFunc is a function that is defined somewhere above the module.exports line. You could also write the exports using:

``` javascript
exports = fooFunc;
```

They do the same thing.

If our fooFunc is in a file named “fooScript.js” then our foo variable above can call the fooFunc() function by using foo.

``` javasript
var foo = require('fooScript');
foo(); // this line calls fooFunc() in fooScript.
```

Optimizations
-------------

What took me a while to grasp is that you can export anything.  A function, a variable, an object.  It really doesn’t matter.  But you have to be aware of some optimizations that Node makes for you.

You see, it would be pretty stupid to process the file every time it was required into another file.  So rather than do that, Node caches the export and assigns that the next time it is required.  If you export an object, the next time you require it, you will get the same object.  So an exported object becomes a singleton.  If you want to be able to create new objects, you are better off exporting the function (or class in ES2015) that creates the object and new-ing it up when you need it.

Your Turn
---------

So, now it is your turn.  If you have not tried using Node.js install it and try a few things.
