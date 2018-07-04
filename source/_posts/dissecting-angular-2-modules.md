---
title: Dissecting Angular 2 Modules
tags:
  - angular 2
  - javascript
  - modules
url: 4149.html
id: 4149
categories:
  - Angular 2
date: 2016-12-20 07:30:00
---

In the new world of Angular 2, and even in the world of Angular.js, you might feel like the concept of a module is the most difficult to wrap your head around. This is especially if you’ve only ever written client side JavaScript code.  Once you've learned why you need a module, the temptation is to use one module for all your code.  I am guilty of doing that myself when I first started.  But, many times using one module for your entire application is the wrong thing to do because it reduces the ability to reuse your code in other modules.  Once you understand why modules exist, you’ll begin to reason about how to use modules appropriately. <figure>![](/uploads/2016/12/image-1.png "Dissecting Angular 2 Modules")<figcaption>Photo credit: [Sappymoosetree](//www.flickr.com/photos/bahkubean/416801559/) via [Visual hunt](//visualhunt.com) / [CC BY-ND](//creativecommons.org/licenses/by-nd/2.0/)</figcaption></figure>

<!-- more --> 

Why Do Modules Exist?
---------------------

When the Angular 2 project started, modules did not exist, even though they had existed in Angular.js.  But as the RC process continued, it became obvious that modules were going to be necessary.  We could have written our code without modules, but the amount of code we would need to repeat to get the same functionality would be dramatically greater. It also often becomes more complex and harder to reason about. So, what exactly do modules get us? If you’ve worked in languages in the past that have the concept of a namespace, it might help you to think of a module as a substitute for namespaces.  They allow us to group similar functionality together, and specify what functionality that belongs to the module can be accessed by the outside world.  For example, I recently wrote a component that all the applications in our organization will be starting out with.  It is composed of multiple components, but I only want the top most component exposed to the developers who will be using it. So, modules allow us to both group code together and encapsulate code so that code that might otherwise be publicly available becomes private to the outside world.

Dissecting Modules
------------------

You may remember that in the [application we’ve been working on](//github.com/DaveMBush/GettingStartedWithAngular2/tree/Step-4) we’ve already created a module.  In fact, when you create any application using the CLI, there will always be this top-level module.

@NgModule({
  declarations: \[
    AppComponent
  \],
  imports: \[
    BrowserModule,
    FormsModule,
    HttpModule,
    GettingStartedWithAngular2RoutingModule
  \],
  providers: \[\],
  bootstrap: \[AppComponent\]
})
export class AppModule { }

The declarations section simply specifies the components that this module owns.  In the case of our app so far, the only component it owns is the top-level AppComponent. The imports section loads modules this module is going to need available in the components it owns.  Many times, the components we need access to are only needed in our templates.  Prior to using modules, we would need to include our components in our TS files just so the templates could access them.  By loading them in the module, we can load modules once even though we may have multiple components that are part of the module that may need them. Since our module is not using any injectables, the providers section is empty.  But that is what the providers section is for.  Any class that we will need to inject in the constructor of other code would be listed in this providers section. Finally, the bootstrap section has the one component this module should load.  This only shows up in the top-level module.  While it is needed, I’ve yet to figure out why.  We’ve already loaded the module, and therefore the component and the tag for the top-level component is in our index.html file, so I can’t see the point of specifying it here yet again. You may wonder, “How does this AppModules get loaded?”  Go back to the root of your app directory and look for the main.ts file.  You’ll see that we loaded it in there.

platformBrowserDynamic().bootstrapModule(AppModule);

You might wonder why this code is not included in the application module.  This is because you might not always want to use platformBrowserDynamic().  If you are using Web Workers to run your code or using Ahead of Time compile, you would use two other methods of bootstrapping the module.  I've only mentioned three here, but there are others. The next property you are likely to see in a module declaration is the exports section:

exports:  \[Edit\]

This tells the module that has imported this module that it can use the Edit component.  We didn’t need this in the app module because the app module isn’t being used by another module.  It is the top most module and really can’t be used by any other module by definition.

Routing and Modules
-------------------

Several weeks ago we took a look at [implementing Lazy Loading by modifying our routing module](/angular-2-lazy-loading/).  What is unique about this situation is that every time you lazy load a module, it becomes the top most module, so once again, there is no real need to export it.

When Should Modules Be Used?
----------------------------

As I said when I started, the temptation is to just put all our import statements that our application is going to need in our app module.  But that seems like an extremely lazy way of writing code.  And when you go to write lazy loaded modules, that’s not going to work so well for you.  So, there are two ways you can approach this. First, you can just create a module for every component you write.  It is probably over kill, but it would be hard to go too far wrong here.  Given the choice between too many modules and not enough, I’d error on the side of too many.  At least it gives you the flexibility do make necessary changes in the future. But the more reasonable approach would be one module per feature and a module for common stuff.  For example, if you are writing a component library, you would probably be safe writing a module for all the components in your library so you only have to import the library module and all your component would automatically become available to you.  You'll want to at least want to have one module per route so that you can lazy load the routes if you decide that is necessary.
