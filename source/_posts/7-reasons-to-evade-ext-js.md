---
title: 7 Reasons To Evade Ext JS
tags:
  - ext js
  - javascript
  - sencha
url: 3972.html
id: 3972
categories:
  - EXTjs
date: 2016-07-13 06:30:00
---

I’ve worked with Ext JS now for a total of 2.5 years.  First with Ext 4.2 and now with Ext 6.x. Here’s my experience, and warning, of why you should avoid this disaster of a framework. ![7 Reasons To Evade Ext JS](/uploads/2016/07/image-1.png "7 Reasons To Evade Ext JS") 

Jack of All Trades
------------------

Master of none! One of the great selling points of using Ext JS is the fact that it comes with “Everything you need” to build a web application.  That would be great if it were true.  But the fact of the matter is, it comes with all of the features you need but the features are all only partially implemented.  I’ve complained publicly several times that Sencha can’t possibly be testing the code they release because it only works in their demos.  If you try to use a feature they have documented as being available, you are likely to find that the feature doesn’t actually work.  How is it possible that you’ve written documentation for how something is supposed to work and yet you can release it without it working properly?  I can understand fringe stuff getting by.  We can’t think of every test.  But when this happens over and over again, you start to wonder what exactly they are testing.

A Wolf in Sheep’s Clothing
--------------------------

When I first started with Ext, the only design pattern they had available was what they referred to as MVC.  It took me two months of playing with the framework before I finally realized that what they were calling MVC wasn’t anything the [Gang of Four](/designPatterns) would recognize as MVC.  I guess if you have a View, a Model and a Controller, you can call it MVC?  It doesn’t matter that the Models define records in a table or that the Controller is tightly coupled to your view.

Sheep Without Legs
------------------

OK.  So when they introduced the MVVM architecture I actually started to have just a bit of hope.  Yes, there were still some fundamental issues I have, but MVVM would make this tolerable.  But here is the issue.  Their idea of MVVM is that you would only need to implement it on a per page basis. Let me try to explain.

### Broken Data Binding

In my ideal world, when I build a new component, I would build that component using the framework the rest of my application is using.  So my component uses MVVM.  Sencha’s implementation gives you a View, ViewController, and ViewModel.  Mostly this looks more like MVC if you ask me but whatever, it has two-way databinding, so we’ll call it MVVM for now.  If you build a component that lives inside another component, the first thing you’ll discover is that binding only works from the top down.  That is, I can bind data at the outer layer and it will get reflected all of the way in to the inner most component that uses it.  But, if you change the data in the inner most component, it doesn’t reflect back up to the outer most component.  I’ve written a hack for this, and there is no promise from Sencha that this will ever get fixed properly, so I guess my hack is safe.

### Broken Controllers

But it gets worse.  While child components can find data in models that are in parent components properly, they can’t find references to functions in controllers in the same way.  This is particularly problematic if you write a component that is a container of other components.  You would naturally want the child components to use the controller from the component that they were declared in.  But if you have an outer component that has your container component as a child and then other components inside of that.  The only way you can control what controller the child most components are going to notify of events is by wrapping the inner most components in their own component with their own controller.  This gets to be awkward when all you want to do is provide an event handler for one control in a column of a grid control.  Again, I have a monkey patch that fixes this, but why did I have to write it? This is just one specific example of my “Jack of All Trades” point that I started with. We won't even address the question of if this is really MVVM or not!

Never Use the .0 release
------------------------

I think most of us now are generally conditioned to be wary of the .0 release of anything that hasn’t been developed using Open Source methods.  There just haven’t been enough eyes on the project to ensure that everything works as it should. But with Sencha, this extends to all of the patch releases at the very least and even into some minor releases. While the 4.0, 5.0, and 6.0 releases were unacceptably broken, we find that every new patch or minor release that comes out afterward breaks something that was working.  We always have to ask, “Can we live with this?”

All or Nothing
--------------

As I said at the beginning, Sencha gives you everything.  That sounds good.  You won’t have to go looking for a grid control, or many other common controls you might want to use. But the bad news is, you can only use controls that were written to be used with Ext.  Which other than what Sencha provides in the framework, doesn’t give you a lot of choices.  Don’t go thinking you’ll supplement Ext with a selection of third party controls.  It’s not going to happen.

Fences Protect AND Isolate
--------------------------

Up until this point in my post, no one can reasonably argue that anything I’ve said is actually a benefit.  At this point we switch to points that may vary based on how well you know JavaScript, HTML, and CSS. You see, the good news, and actually a major selling point to many people, is that you can write a web application using Ext without having to know much, if anything about HTML or CSS.  And for that matter even the amount of JavaScript you need to know is relatively limited. That’s the good news.  The bad news is, if you know anything about any of these, you’ll probably end up frustrated by EXT.  This is because Ext’s JavaScript controls most of the layout.  So if you are used to going into developer tools to tweak the CSS and then applying that to your style sheet, you are going to be very disappointed.  Pretty much nothing you do in developer tools is going to work as you would expect.  And figuring out how to apply those to your code is going to be a lot harder than you are used to.

Their Way or the Highway
------------------------

Once again, many people see this as an advantage.  And once again if you aren’t familiar with how the rest of the JavaScript world does things, this is going to sound fine.

### Sencha CMD

Everything runs through Sencha CMD.  A tool for building all things Ext.  If you want to bundle and minify your code, the standard way of doing this is by using “requires” statements in your code and then running Sencha CMD and have it figure out what you are using and put it all in one bundle. The problem with this is that there are several much better ways of doing this that are available using Node and various NPM packages.  Again, if you are a JavaScript developer, you are going to wonder what Sencha is thinking.

### Ext.define()

Another place where proprietary shows up is in how Ext defines “Classes”.  When it was first introduced, TypeScript was new.  But now, we not only have TypeScript, which does much of what Ext does and some things it doesn’t, but we have an evolving JavaScript standard that I’m afraid Sencha won’t be able to keep up with.  They already discourage the use of ‘use strict’;.  Once again, there is only one place where this will get you in trouble, and the work around actually produces more efficient code.  But still, the point is, Sencha is relying on ECMA Script 3 standards while the world has largely moved on to ECMA 2015 and beyond. Anyhow, my point here is that Ext is not just a framework but also functions, largely, as its own language.  Not quite as much a fork from the standard as Coffee Script, but also not nearly as close to the JavaScript spec as TypeScript.  So while it is still JavaScript, if you are a JavaScript programmer, it isn’t going to feel quite like JavaScript to you.

### Themes

The final place you will find “Proprietary” lurking is with the Themes.  There are several really good CSS frameworks out there.  Sencha uses none of them.  And while the syntax they use for creating themes has been SASS up until Ext 6, now they even have their own proprietary SASS compiler.  Watch out here because they are still using the SASS extensions so you are likely to make some assumptions here that aren’t true because, once again, they’ve only implemented enough of the SASS engine to do what THEY need to do.

VB All Over Again
-----------------

Every time I hear someone praise how great Ext is, it is normally because it has everything you need out of the box and allows you to get stuff done quickly.  Basically the same argument for using Visual Basic back in the day.  And yet I learned to never take a VB job because it almost every instance, while it was possible to write well structured code in Visual Basic, it was generally so difficult to do that the code I would be maintaining would need to be rewritten in order to make any sense of it. Ext suffers the same issue.  There is nothing in Ext to force you to write well structured code.  The code I have had to maintain has almost always followed every anti-pattern known to man.  In this case, this isn't Sencha's fault directly other than the fact that the only reason my code tends to be cleaner than most is because I'm more likely to code a fix to an Ext bug than I am to work around the problem with an anti-pattern. In comparison to other frameworks that are available, if all you want is a tool that will get you a semi working application quickly, and you don't care so much about having to rewrite it when you need to change it in some way, Ext is your tool.  If on the other hand, you care about design and you want to be able to maintain what you've written, you should look elsewhere. Remember, if it sounds too good to be true, it probably is.
