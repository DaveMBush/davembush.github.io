---
title: 'Ext JS 6 by Sencha - The Good, The Bad, The Ugly'
tags:
  - ext js
  - javascript
url: 3870.html
id: 3870
categories:
  - Javascript
date: 2016-04-07 07:30:00
---

Long time readers may remember that I started using Ext JS about 3 years ago.  At the time, I was using version 4.2.2.  I recently started a new contract where they are using Ext JS 6.0.1.  I have to say, this version solves a lot of the architectural issues I had with the 4.x series.  But, there are still problems. Since I’ve provided an evaluation of [Angular 2](/angular-2-first-impressions-compared-to-angular-1/) and [React JS](/react-js-and-associated-bits/), I thought providing an evaluation of the current version of Ext JS would be appropriate since these three seem to be the main players in the corporate world. <figure>![](/uploads/2016/04/image.png "Ext JS by Sencha - The Good, The Bad, The Ugly")<figcaption>Photo credit: [sanbeiji](//www.flickr.com/photos/sanbeiji/5606497634/) via [Visual Hunt](//visualhunt.com) / [CC BY-SA](//creativecommons.org/licenses/by-sa/2.0/)</figcaption></figure>

<!-- more --> 

Ext JS - The Good
-----------------

### MVVM

I’ve always had three major complaints about Ext JS.  Of the three, the fact that Ext JS is nearly impossible to test is the one that drove me away.  In fact, I almost didn’t interview for the contract I have now because they are using Ext JS.  This is because the 4.2 version that I was using implemented what they called the MVC framework.  The problem is, the MVC framework they implemented was not anything [the Gang of Four](/designPatterns) would recognize.  Once I realized that what they were calling MVC wasn’t really MVC, I was able to learn how to use the product much better. But being the TDD guy that I am, I was always frustrated by their implementation of MVC because in order to test anything in the Controller, I had to have the view available.  And while I tried several ways of mitigating this problem, I was never completely satisfied with the solution.  I ought to be able to test my controller without a view, or if I have to have a view, it should be some sort of fake view, or be able to render into a fake DOM like React JS does. But, in Ext JS 6, they’ve provided an alternate framework.  This time it is also more accurately named.  They have provided an MVVM implementation.  In the View, you provide your layout, declarative syntax to access the View’s state from the ViewModel and to specify the event handlers using listener blocks that tell the view what methods to call in the associated ViewController class. In the ViewController, your methods can access the ViewModel by calling getModel() and can set the state of the view by calling the ViewModel’s set() method.  Once this is done, the View can update using the ViewModel’s new state. What this means for testing is that I can test without the View by overriding the ViewContoller.getModel() method to return the ViewModel.  Run my test for a method and check the state of the ViewModel.  Look Ma, no View!

### Everything You Need

One of the biggest selling points for using Ext JS is that just about everything you could need is provided for you in once product.  Unlike Angular or React JS where one project provides the framework and another project or projects provide components, nearly everything you are going to need for your application is provided out of the box.  This is not to say that there aren’t third party providers for Ext JS, but the need for them is very limited.

### Consistent Rendering

One of the major attractions Ext has offered is that you don’t need to worry about cross browser rendering issues.  If you still need to support REALLY old browsers, this may still be a big selling point for you.  I think this will matter less in the future as the browsers continue to stabilize around standards.

### Responsive/Adaptive

Even though Ext JS uses a none standard way of rendering controls (see below) they do manage to achieve Adaptive and Responsive designs.

### Ability to Control DOM Manipulation

Finally, if you are having trouble achieving performance with the current way you are rending DOM changes, you will be happy to know that Ext JS does provide a way of turning of rendering to the DOM while you make all the changes and then turning it back on to do the final rendering.  But, at least my last usage of this, indicates that it doesn’t really turn off ALL DOM manipulation.  If you are inserting new DOM elements, those go out to the screen.  All Ext JS really does is to turn off their layout code.

### Who Ya Gonna Call?

One of the strongest reasons many organizations choose Ext JS is because the price of the license gives you access to Sencha support.  Companies I’ve worked for have used this for everything from “My code doesn’t work, what am I doing wrong?” and actually getting an answer to “I think you have a bug here.” and getting the bug fixed.  Kind of a private StackOverflow with direct access to the programmers who wrote the framework.

Ext JS - The Bad
----------------

### Lock In

If you decide to use Ext JS, you are really making a much more significant commitment than if you were choosing to use either Angular or React.  With either of those two, I can write standard JavaScript and I can mix and match several different existing frameworks.  Since just about everything in Ext JS is proprietary, mixing and matching is not only frowned upon, but they warn against it.  If you are using Ext JS, you are going to use ALL of Ext JS for everything.

### Use Strict

Standard JS best practice recommends placing “use strict”; at the top of you IIFE block to protect you from making stupid mistakes.  Unfortunately, you can’t do this in your Ext JS code without having to work around the problems it produces.

### String Based

Ext JS is probably the most string based language I’ve ever seen.  While they now have plugins for some of the more popular IDEs that mitigate against the risk this imposes on your code, in terms of good solid JavaScript, there are much better ways of writing code than what Ext JS forces you into.

### Nesting Issues

As I mentioned above, Ext JS does their own layouts in order to achieve a presentation that will look the same regardless of what browser it is running on.  However, the cost of this is that if you nest components too deeply, rendering your view or changes to your view, will take significantly longer than anyone is willing to wait around for.  So, to get around this, you end up writing sub optimal code from just about every coding principle in existence.  Specifically, DRY and SRP are difficult to achieve using Ext JS views.

### Version X.0.0 is Always Broken

I’ve complained about this publicly before.  But it seems to me, and everyone else I talk with that has used Ext JS that every .0.0 version is buggy.  Things that used to work in the previous version no longer work.  Despite the assertion from Sencha that they have thousands of tests.  I always wonder what kind of code coverage they have and if they have a test that covers every feature for every component they have documented.

Ext JS - The Ugly
-----------------

### Ugly HTML

There is a lot that is ugly about Ext JS, but nothing is more visibly ugly than the HTML it produces.  This is because, in order to produces a view that will render on any browser, they’ve resorted to using HTML tables to wrap just about every standard control.  This is getting better.  There is less HTML generated in Ext JS 6 than there was in Ext JS 4, but it is still relatively ugly. And that whole nesting issue could go away tomorrow if they would give up on trying to control the rendering of the view through JavaScript.  Why do with JavaScript what CSS was designed to do and does MUCH better?!

### SASS isn’t SASS

Up until version 6, Sencha’s theming engine used standard SASS.  With version 6, they’ve dumped standard SASS for their own implementation that mostly does what SASS does but has a few embellishments that aren’t all bad, except for the fact that they still kept the SASS extensions for the files and the syntax is mostly the same.

### None Standard JavaScript

But of all the issues I have with Ext JS 6, the one that bugs me the most is that their framework provides something that runs on JavaScript but really isn’t JavaScript.  They have their own way of declaring a class.  Their own way of instantiating a class.  Their own requires engine.  Their own bundling and minification engine. And since I can’t even use “use strict”; in what they have now – something that has been around long enough that it should be supported by every seriously used framework in existence – it makes me wonder what future embellishments to the JavaScript language we won’t be able to use because Sencha thinks they have a better idea. Will I be able to use the “class” keyword in the future instead of Ext.define()?

### None Standard Build Tools

Not only does Ext JS use none standard JavaScript, but they are using their own proprietary build tools to deploy the final applications.  Along with using their own version of SASS, they also have their own implementation of bundling and minification.  Why not use gulp or grunt and allow us to bundle our apps our way?  Oh, right, they have their own implementation of requires too.  And now they want to sell us proprietary testing tools.

Conclusion
----------

So, is Ext JS for you?  That’s a good question.  You’ll need to evaluate if the good parts outweigh the bad parts.  It isn’t like either Angular or React have everything.  There is no perfect choice.  There is the best choice for you and your organization.
