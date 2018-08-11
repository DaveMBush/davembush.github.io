---
title: Angular 2 Lazy Loading
tags:
  - angular
  - lazy loading
url: 4109.html
id: 4109
categories:
  - Angular
date: 2016-12-06 07:30:00
---

Last week when we took a look at [Client side Routing](/adding-client-side-routing-to-angular-2/), I mentioned that one of the reasons you’d want to implement a component in its own module is so that we could lazy load the component and its dependencies. This week, we want to dig into how to implement lazy loading in your Angular 2 application. <figure>![](/uploads/2016/11/image-5.png "Angular 2 Lazy Loading") Photo via [jordan](//pixabay.com/en/users/jordan3600-400129/) via [Visualhunt](//visualhunt.com/)</figure>

<!-- more --> 

What Is Lazy Loading?
---------------------

Imagine that you’ve written an application that is divided into four main sections.  The people using the site may only use one or two sections of the site at a time, or at all.  Is it fair to make them download the entire site? Or maybe you’ve written a monster site.  Wouldn’t it make more sense to download only what you need as you need it?  The perceived performance of a site written like this far exceeds the performance of a site that requires you to download everything at once. And so, Lazy Loading was developed as a way of solving these issues and others.  The idea is, rather than downloading everything, download only what is needed when it is needed.

The Code
--------

Remember that last week we implemented a View module and an Edit module.  We called them from our router by importing the modules and telling the router to load the components when a route was specified. Because we are no longer loading the modules as part of our main application, we are going to go into the `app-routing.module.ts` file and remove all of the references to the components and the module.  Instead, we are going to load the module using the `loadChildren` property in our route array.

import { NgModule } from '@angular/core';
import { Routes, RouterModule } from '@angular/router';
// import { ViewComponent } from './view/view.component';
// import {ViewModule} from "./view/view.module";
// import {EditModule} from "./edit/edit.module";
// import {EditComponent} from "./edit/edit.component";

const routes: Routes = \[
  { path: '',
    loadChildren: './view/view.module#ViewModule'
    //component: ViewComponent
  },
  { path: 'Add',
    loadChildren: './edit/edit.module#EditModule'
    //component: EditComponent
  },
  { path: 'Edit/:id',
    loadChildren: './edit/edit.module#EditModule'
    //component: EditComponent
  }
\];

@NgModule({
  imports: \[
    RouterModule.forRoot(routes)
    //ViewModule,
    //EditModule
  \],
  exports: \[RouterModule\],
  providers: \[\]
})
export class GettingStartedWithAngular2RoutingModule { }

You’ll notice the format of the loadChildren string is:

1.  The path to the module (without the file extension)
2.  hash
3.  The class in the component that is exported

But, there is nothing in the code that tells us what component is supposed to load.  This is because the job of deciding what component to load has now been delegated to the module. This is the part that remained a mystery to me for quite a while.  I would look at the code demos but never saw the next part.  Maybe this will save you some of that trouble. In the view module, add this line to the imports array:

 RouterModule.forChild(\[
    {path: '', component: ViewComponent}
\])

Notice that we are using forChild here instead of forRoot but otherwise this looks the same as what we originally had for the View component prior to implementing lazy loading. We can implement a similar line in the EditModule, except the component will be EditComponent.

RouterModule.forChild(\[
    {path: '', component: EditComponent}
\])

Under the Hood
--------------

Build the system using `ng build` because I want to show you what is happening under the hood. Once you’ve built the system, look in the `dist` directory.  You’ll see a `0.chunk.js` and a `1.chunk.js` file.  These files hold the module and dependencies that we’ve lazy loaded. If you load the application with the developer tools loaded and look at the network tab, you’ll see that one chunk is loaded immediately, for the view.  The other is loaded when you navigate to the add or edit path. The code so far can be found at [https://github.com/DaveMBush/GettingStartedWithAngular2/tree/Step-4](//github.com/DaveMBush/GettingStartedWithAngular2/tree/Step-4 "https://github.com/DaveMBush/GettingStartedWithAngular2/tree/Step-4")
