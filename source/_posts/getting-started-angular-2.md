---
title: Getting Started with Angular 2
tags:
  - angular
  - javascript
url: 4074.html
id: 4074
categories:
  - Angular
date: 2016-10-25 06:30:00
---

Angular 2 is finally released.  But the biggest problem with learning Angular 2 is that it is a “Choose your own adventure” kind of framework.  And while React has a similar problem, I think Angular has out done them.  This means that you can learn bits and pieces of Angular 2, but it will be a while before you get a cohesive understanding of what choices you need to make, which choices are the right choices, and why all of this matters. And all of this is even more difficult if you are a relatively new programmer.  I’m talking those of you who have less than 5 years of experience and even some of you who have less than 10 years of experience. So, what I thought I’d do to address this very real problem is to assemble a very opinionated Angular 2 tutorial.  Over the next several weeks I plan to show you how to create a simple CRUD application using Angular 2 in a way that will scale up to larger projects.  While I may mention some of the other options along the way, what you’ll end up with is the “right way.”  OK.  To be fair, most of what I consider “right” is opinion.  Some very smart people disagree with me.  But, some other very smart people agree with me too. <figure>![](/uploads/2016/10/image-2.png "Getting Started with Angular 2")<figcaption>Photo credit: [mikecogh](//www.flickr.com/photos/mikecogh/11300349426/) via [VisualHunt](//visualhunt.com) / [CC BY-SA](//creativecommons.org/licenses/by-sa/2.0/)</figcaption></figure>

<!-- more -->  Here’s where I think we are going with this.

*   Getting the project set up.
*   Building, Running, Testing
*   Adding in external packages
*   Client Side Routing
*   Building Components
*   Using Reactive Programming
*   Managing Application State
*   Accessing the Server
*   Using Web Workers for more Responsive Applications
*   Ahead of Time (AoT) Compiling
*   Server Side Rendering

In this article, let’s focus on just getting a basic application up and running.

Prerequisites
-------------

If you haven’t installed Node yet, I suggest you do that now.  [Here’s an article on how to install Node if you are new to Node](/you-can-start-using-node-today/).  Even if you think you know how to install node (“How hard can that be?”) read the article.  You might learn something.  I would recommend installing Node 6.x as of this writing or whatever the latest version is when you are reading this. You’ll also want to get an editor that has good support for TypeScript, HTML, and CSS.  I’ve heard a lot of good things about VS Code, but the editor I use is WebStorm.  If you are a .NET programmer, Visual Studio will get you kind of close.  If you can’t or won’t switch to WebStorm for your JavaScript development, at least get the ReSharper plugin for Visual Studio.  You’ll also need plugins to enable Node and TypeScript development from within Visual Studio if you haven’t installed them already. For those of you in the Java world using Eclipse.  Eclipse has the worse JavaScript support I’ve ever seen.  I understand that it is familiar, but just about any other JavaScript editor will be better. Anyhow, I’m using WebStorm.  If you are using another editor, you are on your own.

Angular CLI
-----------

Now that you have Node installed and you have a reasonably good JavaScript/TypeScript editor, the next thing you will want to install is the [Angular CLI](//github.com/angular/angular-cli).  As of this writing, the Angular CLI is at Beta .17 so I can understand that you might be hesitant to use it.  But it is done enough that we can use it to get our project going with MUCH less effort than if we did it by hand.  And hopefully, the parts we need will get completed by the time we need them. There are a few other practical reasons for using the CLI rather than coding it yourself.  First, the CLI conforms to the Angular 2 style guide.  This was developed by the team that wrote Angular 2.  Who better to tell us what the code should look like?  And while I may not always agree with some of the recommendations, I understand why they are there and I’m willing to live with them.  Hopefully, organizations coding Angular 2 applications will conform to these conventions so that anyone who writes Angular 2 code will already know them as they move from one organization to another. The instructions are on the web site for installing.  But here is what you need to do in a little more detail. First, install the angular cli globally.

> `npm install -g angular-cli`

By the way, when you run `npm` commands, or `ng` commands later on, you’ll do that from the command line. The next thing you’ll want to do is create a new project using the Angular CLI.  This is where things are not as clear as I would have liked.  Probably because their development environment looks different than mine. The official documents say to run these commands

> `ng new PROJECT_NAME cd PROJECT_NAME ng serve`

where PROJECT_NAME is the name you want to give your project.  This project name will become a sub directory under the directory you executed the command from.  But what if you want to create a new project first, using your editor, and then you want to run `ng new`?  My first attempt was to run ng new using the directory where my project existed.  But that just gave me an error message that said the directory already existed.  Yeah, I know. Then I tried running `ng new` without the directory name inside the project directory.  The result of that was being told that I forgot a parameter. Well, what about using `ng new` with “.” as a parameter, meaning “current directory”.  Nope, “.” is not a valid name. It took me a while to figure this out. But you can execute `ng init` from within the directory your existing project is in and it will scaffold out your application in your current directory.

NG SERVE
--------

One of the commands you’ll see above is `ng serve`. You may wonder what this is and why you would want to run it. Don’t I just deploy my application on my own server? Well, yes, eventually you will. But while you are developing the application you will want or need to test it locally. Since Angular 2 requires a build and packaging step, you’ll want to automate this as much as possible. So, before you toss the idea of using `ng serve` out, let’s take a look at what this gives you. First, `ng serve` gives you a web server to run your application.  By default, this runs on [http://localhost:4200](//localhost:4200).  You can try this now. Oh wait!  What’s it doing?  It looks like it is doing way more than starting a web server. Well, yes, it is.  You see `ng serve` automatically compiles all of your typescript files and bundles all of your resulting files together every time a file changes.  The first time you run `ng serve`, it will do this because everything has changed.  But it gets better, it will also refresh whatever browser is looking at the application so that your browser will always reflect whatever changes you’ve made in your source code. There are a few specific cases where this won’t work, but generally this works out pretty well. If you make a change and it doesn't get reflected as you would expect, try restarting the server. That will probably fix the issue. The other thing you can do is that you can configure the server to proxy request to another server.  This would be useful if you want to pull data from your Java, ASP.NET, or other platform while still running ng serve for the client side development work. If everything is working so far, you should be able to pull up your browser and run the application on [http://localhost:4200](//localhost:4200) and see a message “app works!”  It really is just the barest of all possible applications.

What Else?
----------

If you are curious like I am, you may have taken a look around the file structure to see what was installed.  One directory you may have noticed is the “e2e” directory.  This is where your end to end tests go.  You can run these using the command

> ng e2e

Just make sure you have already run ng serve in another window. You can also run the unit tests that are in *.spec.ts files by running the command

> ng test

This will run the tests using Jasmine in a Chrome browser. The final thing to note about the CLI is that it uses Webpack instead of System.JS to manage bundling and minification.  As of this writing, the tour of heroes demo still uses System.JS so this might be a point of confusion for you.  The good news is that Webpack is a bit more straight forward than System.JS and I believe you’ll find it a lot easier to use.

Follow Along
------------

The result of step one can be found on my GitHub account: [https://github.com/DaveMBush/GettingStartedWithAngular2/tree/Step-1](//github.com/DaveMBush/GettingStartedWithAngular2/tree/Step-1 "https://github.com/DaveMBush/GettingStartedWithAngular2/tree/Step-1")
