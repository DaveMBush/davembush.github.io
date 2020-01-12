---
title: Angular 2 Thoughts
tags:
  - angular
  - javascript
url: 4056.html
id: 4056
categories:
  - Angular
date: 2016-10-04 06:30:00
---

I was asked this past week what my thoughts were on Angular 2. I wrote early on about my impressions of Angular 2 when it was barely done enough to review. But now that I’ve been working with it for a while and know a bit more, what I want to discuss is more along the line of what it means to the average developer and, more importantly, organizations that are planning to use it.

![image](/uploads/2016/09/image-3.png "image")

Options
-------

While like Angular 1, Angular 2 provides us with most of what we need to build an application. Angular 2 also provides us, out of the box, with very Angular 1 ish ways of putting together an application, complete with modules. What Angular 2 provides that isn’t quite so obvious going into your development is options.

In angular 1, we had options at the GUI level. And while this is still true for Angular 2, this is not the only place where you will see options.

In general, you can split your options into, 1) do you want to do things the Angular 1 way, or 2) would you like to use an approach that looks more like React.JS? No place is this more obvious than with how you deal with forms. You can create forms with a declarative syntax, like you’ve done with Angular 1, using [Template Syntax](//angular.io/docs/ts/latest/guide/template-syntax.html). But, in my opinion, a much better way to create forms is with a more reactive approach using the FormControls and FormGroups classes. If all you are doing is just displaying data, you’ll probably find that using the template syntax is all you need. But once you start working with complex forms that accepts data input and implements validation, you’ll probably want to move toward a more reactive approach. The added benefit is that more of you code will be testable.

The next option you are going to have is figuring out how to move data around your system. Most of the literature is going to suggest you use a data flow that looks a lot like what you were doing in Angular 1. But there is nothing stopping you from using Flux, Redux, or even RxJS instead. And here again, my recommendation would be that you learn these because I think you’ll find that your system will end up being much easier to reason about than the old MVVM stuff you may be used to.

Lazy Loading
------------

There are several places where we had to make choices in Angular 2 where the feature has been built into the framework. One of these places is lazy loading. Why load all of your JavaScript up front? Load what you need when you need it. Angular 2 makes this easy with Lazy Loading and the choice between using WebPack (the default) or System.JS.

I’ll warn you though, as of this writing, Lazy Loading only really works when using System.JS unless you want to spend a lot of time tweaking your webpack config file.

Angular 2 CLI
-------------

Command Line Interfaces seem to be the cool new kid on the block. You aren’t a real framework unless you have one. While the Angular CLI is not quite baked yet, I can see how this is going to make writing Angular 2 apps much easier. There are a lot of moving parts involved in getting even the most basic of applications up and running. The Angular CLI makes starting your first application REALLY easy. It even hides all of the WebPack internals while allowing you to add your own config file if you need to. Once they have the routing bit re-implemented, it should make using Lazy Loading with WebPack much easier.

The other thing using the CLI will do for you is that you will automatically start following the [coding standards](//angular.io/styleguide) that the Angular team have developed.

Angular 2 Components
--------------------

Right now, there aren’t a lot of options available for Angular 2 for custom components. While Kendo UI has been the defacto standard for Angular components in the past, Telerik is in the middle of rewriting [Kendo UI for Angular 2](//www.telerik.com/blogs/kendo-ui-for-angular-2-r3-roadmap) (and React.JS). I haven’t seen any movement in the Angular UI camp to support Angular 2. Angular 2 Material has a few components that seem ready, but they are all relatively simple. The only vendors that seems to have a complete Angular 2 package is [Wijmo](//wijmo.com/angular2/) and [Prime Faces](//www.primefaces.org/primeng/). I haven’t tried them and I’m not endorsing them. I’m just reporting what I’ve found. There are a few standalone components here and there, but if you are looking for a set of components you can just use from one source, I’m afraid you’ll have to wait. This isn’t necessarily a bad thing. It gives you time to properly learn Angular 2.

But what about the Angular 1 to Angular 2 bridge? In my mind this is way more trouble than it is worth. We’ve waited two years for Angular 2, I think we can wait just a bit longer for a set of components that we can use with it.

What Angular 2 Means for You
----------------------------

The main difference between Angular 1 and Angular 2 is that just about any Script Kiddie could pick up Angular 1 and get something done. Angular 1 was developed during a period of JavaScript history when JavaScript had not quite reach the level of “serious programming language.” But now, JavaScript has not just reached that level, but several very serious frameworks have been developed.

What I notice as I review the JavaScript landscape is that we’ve moved from just getting stuff done with little to no planning, to treating JavaScript as a first class programming language that requires, and even demands that we adhere to a set of programming best practices that we would use for any other language. These include things like naming conventions and design patterns. For you to write code well in this new universe, you will need to understand what these design patterns are, why they exist and how to implement them well. If you continue on your merry script kiddie way, you will soon find yourself out of work.

Further, if you think you know Angular 2 just because you know Angular 1, or you think Angular 2 will be easy to pick up because you know Angular 1, you are in for a very big surprise. Many of the concepts are the same. But since Angular 2 has so many options, you should learn the options well so that you can make an educated decision about which option to use.

What Angular 2 Means for Organizations
--------------------------------------

Similarly, if you are an organization that is planning to move to Angular 2, don’t expect your programmers to just pick up and move to Angular 2 overnight. Give them time to learn it. My recommendation is that you make learning it part of their job while they continue to use Angular 1 for the product you are trying to complete. We all have time pressure, so you need to factor learning into the schedule.

But Angular 2 may also mean you need to find additional programmers who already have the skills and can transfer the knowledge to your team.
