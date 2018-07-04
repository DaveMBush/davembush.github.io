---
title: Why more Angular Modules are Better than One
tags:
  - angular
  - modules
  - ngmodule
url: 4548.html
id: 4548
comments: false
categories:
  - Angular 2
date: 2018-01-16 06:30:00
---

I recently reviewed some Angular code that uses one module.  The AppModule. To manage the entire code base.  And, this isn't tiny code base.  The main excuse I've heard for this is that the code originated during the beta cycle, prior to NgModule being added to the framework.  I call it out as an excuse because once it was added, it was clear that we needed to have more than one module.  The fact that this code base doesn't have more than one module shows a disregard for doing things right over doing things fast.  In the best case, it shows ignorance. But, the larger question this code base raises for me is this: "Why are more modules better than fewer modules?"  After all, using one module obviously works.  Isn't the fact that it works sufficient enough? And here are three really good reasons to use more modules. <figure>![](/uploads/2018/01/2018-01-16-1.jpg "Why more Angular Modules are Better than One")<figcaption>Photo credit: [goodrob13](//visualhunt.com/author/1d5a2d) on [Visualhunt.com](//visualhunt.com/re/0a04dc) / [ CC BY](//creativecommons.org/licenses/by/2.0/)</figcaption></figure>

<!-- more --> 

Avoid Component Collision
-------------------------

For discussion purposes, let's say you have an application with two routes.  Each route allows you to edit different kinds of content.  You may be inclined to create some child components in each route to assist with your functionality.  In the process of doing this, you may end up with two components that have the same name.  Both as a class and as a selector.  If you are including everything in one mega module, you will need to artificially give the components different names.  However, if each route has its own module, you can declare the components in the declaration section of each module with the same name and each route will be able to call the appropriate component.  Components aren't shared across modules until you export the component.  In that case, you would also want the component to live in a shared module where we would give it a more meaningful, generic, name that made sense as a shared module.

Lazy Loading
------------

Once you've created modules for each of your routes, the next logical step in your development effort will be lazy loading the module.  Lazy loading provides more advantages than just the ability to load only what you need when you need it.  That's just the most obvious gain.  Lazy loading also provides a separate context for @Injectables.  Once again, just like having the ability to isolate the components, by using modules in conjunction with lazy loading, we have the ability to have route specific @Injectables with the same name and each route/module will behave appropriately. There is one caveat.  You can't provide an @Injectable in a lazy loaded module and an application level module and expect things to work correctly.  And, if you are still using a framework like NgRX 2 that needs to have access to services, you'll need to make your services globally available.  This is one of many reasons why I believe you should upgrade to NgRX 4 as soon as is possible.  This allows you to take full advantage of the Angular Lazy Loading capabilities and all their benefits.

Cleaner Code
------------

Even if you've never seen a major application with one module, I'm sure you can imagine what a mess that module is.  I don't think I really need to say much more about this. For more on Angular Modules, I recommend [this FAQ that the Angular team put together](//angular.io/guide/ngmodule-faq). [This Medium article](//medium.com/@cyrilletuzi/understanding-angular-modules-ngmodule-and-their-scopes-81e4ed6f7407) is also pretty good.
