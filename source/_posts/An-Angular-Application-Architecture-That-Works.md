---
title: An Angular Application Architecture That Works
date: 2020-01-01 11:55:53
tags:
  - angular
  - ngrx
  - functional
  - reactive
  - mvp
---

The purpose of this article is to specify a working seed architecture for most enterprise Angular applications.  This architecture aims to achieve the following goals:

- Ensure that all parts of an application have a home so that anyone can pick up any application that uses this architecture and modify the application without having to learn where everything lives.
- Reduce the overall complexity of any one application by using well-established design patterns that work within the Angular ecosystem.
- Reduce the number of bugs introduced into an application by reducing the need for duplicate code.

<!-- more -->

## Guiding Principles

- Largely adhere to the [Angular Style Guide](https://angular.io/guide/styleguide)
- Prefer Functional/Reactive programming over Imperative/Object Oriented programming.
- Prefer Composition over Inheritance when using Object Oriented programming
- For Object Oriented code, and where it applies, Functional/Reactive code, make your code S..D
  - Adhere to the Single Responsibility Principle:
    - Keep the size of the individual files (html, css, ts) small
    - Separate file for each class, function, interface, enum, etc.
    - Keep the cyclomatic complexity of functions and methods small.
  - Keep your code DRY
  - Because we prefer Composition over Inheritance, the OLI part of SOLID don't apply.
- Prefer composition over child routes.

## Directory Structure

Let's start with the directory structure our applications will use.  To start with, the directory structure outlined in the [Angular Style Guide](https://angular.io/guide/styleguide) is used. There are minor modifications that still obey the general principles outlined in the style guide.

``` text
projectFolder
+- src
  +- core
  +- dialogs
    +- dialog1
      +- helper-component-1
      +- helper-component-2
      main dialog files go here directly
      under the dialog directory
        - html, css, ts
        - dialog specific NgRX code
    +- dialog2
      +- helperComponent3
      main dialog files go here
    +- etc...
  +- routes
    +- route-1
      +- supporting-component-1
      +- store
        supporting route specific NgRX files go here
        these include actions, effects, services, reducers,
        and selectors.  Each set of files get their own
        directory. Each extension is:
        - *.actions.ts
        - *.effects.ts
        - *.service.ts
        - *.reducer.ts
        - *.selector.ts
        route store slice module goes here
      route1 components go here
      route1 module goes here
    +- route-1-subroute-a
      similar to above.  This is only IF you have sub-routes.
      I strongly advise against them.
  +- shared
    +- components
      only components that are shared between routes go here
    +- store
      only store files that are shared between routes go here
    +- services
      only services, if any, that aren't used by your store go here
```

What is slightly different from the style guide is that a separate directory is created for dialogs and routes as these will be where my top-level components will live.  Within a route or a dialog, everything that route or dialog needs should live under it.  In the case where a class is needed by multiple routes or dialogs, these files live under shared.

The same apllies to Angular forms. A separate directory is created that houses form level components and the corresponding but separate NgRx store slice. To keep with the principals of single responsibility, the form and its store only handle the state of the form fields. The

As a guiding principle, you should think of each route as a stand-alone application with its own module.  It should be able to run on its own using the classes, functions, etc from either its directory and sub-directories or the classes, functions, etc in the shared directory.

This leads us to Lazy Loading

## Lazy Loading

While it is possible to create an application where the routes are all specified in one file, for large applications, performance will eventually be impacted.  You can save yourself a lot of trouble and reduce the cognitive load necessary to digest one file with all the routes if you commit to lazy loading up front.

### Routes

What this means from a practical perspective is that each route will be loaded in as it is needed. One of the side benefits of this is that the files necessary to run each route will only be loaded as they are needed. This will decrease the time it takes to load the application.

You will, however, need to ensure that each route can be loaded directly because sometimes taking the expected path will load modules that you forgot to load as part of the route.

### NgRX

Similarly, you should make use of feature stores in NgRX so that you can dynamically add in store slices with each module rather than loading them all in when the application loads.  Done correctly, this will also reduce the cognitive load around your store structure as you will only need to be concerned about the store structure for each route.

For consistency's sake, I create feature slices in my shared folder for NgRX slices that are shared rather than load them all at the application level. Again, this reduces the cognitive load necessary to understand any one part of the application. It has the added benefit of making it easier to move slices of the store from a route feature to a shared feature because the structure remains essentially the same.

## Components

A large part of the front-end code is written using components which include the route, controls, and components that encompass blocks of components.

The following addresses how to create components in a way that can be maintained.

### Keep em Small

One of the main problems I've seen in my career with all code, but especially components, is that we try to put all the code for a page all in one place. We don't even think of the third category of components.  My advice for your template code is that once it has gotten past the point of code that can't fit in a file that is 150 lines long and 80 characters wide, or has nested to a point beyond 6 elements, you should consider breaking them out. There may be a few exceptions, but this metric will keep your code easy to understand.

One way you can reduce the nesting without a lot of effort is to style the @host element of the template rather that using a containing DIV tag around the other elements that are the functional components of your component.  Other than because it reduces nesting, it also eliminates a layer of HTML that needs to be rendered. Since we are going to favor lots of small components over a few large components, this will, ultimately, impact the performance of your application.

Another way you can reduce nesting is by recognizing patterns in your code and extracting them into their own component.

Finally, consider using for your templates is cyclomatic complexity.  Codelyzer has a rule you can add to your linting.  I'd set this to no more than 10.  Cyclomatic complexity measures how many paths there are through your code.  Once you've added if, switch or for loops in your template, you've introduced some cyclomatic complexity. If you keep to the metrics I've already mentioned, you should never hit the cyclomatic complexity metric.

### Component Services

Let's talk about the purpose of components for a bit. The point of components is to allow the end user to interact with the program. In practical terms this means it has two jobs:

- Display data that is meaningful to the user
- Allow the user to issue commands that either manipulate the data or take them to another screen.

This is true for whatever programming environment you are using.

I've been programming now for 32 years and I still see programmers who try to make the component, or the view as they are sometimes called, do something else.

This also means that business rules have no place in a component.

Having said that, it is often true that to get the data to display we often need to introduce logic into our component somewhere. Once we've introduced logic, it would be great if we could also write unit tests for that logic without a lot of pain.

If we put this code in the TS file that is our component, this means that in order to test the logic we need to scaffold the component.  Wouldn't it be better or at least easier, if we didn't need the component at all?

This is where component services come in.  If you are familiar with the Model View Presenter design pattern, this is an adaptation of that.

You can read details of how this is implemented on the following two sites:

- [Model View Presenter, Angular, and Testing](https://davembush.github.io/model-view-presenter-angular-and-testing/)
- [Model View Presenter with Angular](https://indepth.dev/model-view-presenter-with-angular/)

The basic idea is that you leave the component as the thing that only presents the data and receives notifications from the user. Any logic that is needed goes into an Angular service that is registered with the component using the viewProviders property of the @Component decorator.

### Smart/Dumb

Broadly speaking, Angular components can be classified as "Smart Components" or "Dumb Components".  Smart Components get their data form the outside world and pass data out to the outside world.  In the architecture defined here, that will be from the NgRX store via selectors and into the NgRX store using Actions.

Dumb Components render data they are passed via the @Input() decorators and fire events back up to the containing component via @Output() decorators.

A lot of people understand these concepts but misunderstand how they apply to an Angular application. You can't follow these rules simply by wrapping your Dumb Components in Smart Components. While this technically obeys the definition, the result is code that is hard to understand. What we want is a central location in our code that is always responsible for getting data to and from the store.

We do this by specifying that our Routes are the Smart Components.  This includes child routes, if you have them.

I've found that it is often easier to make a dialog a smart component as well, so if you find this is easier than passing the data that the dialog needs to the dialog, you can make dialogs smart as well. This is why we have a `dialogs` directory and a `routes` directory as immediate children of the `app` directory.

### Forms

Technically, there are two ways of programming forms using Angular. The first way uses the old 2-way data-binding model and is called "Templated Forms." These were popular in AngularJS (aka Angular 1).  The second is new to Angular (aka Angular 2+) and is called "Reactive Forms". Our architecture will use the newer model because it conforms to the guiding principle of keeping things as Functional as possible and removing as much logic from the component as possible.

There are several implications of how we program our forms related to this.

First, you will define your FormGroup(s) in the Route's component service.

Second, in order to keep your templates small, you may be a need to put bits of your form in child components. You will find that each child component needs its own `formGroup` attribute around the `formControls` that it is responsible for.  The best way to do this is to pass the `formGroup` down from the router into the child components and then assign that variable to the `formGroup` attribute in the child component's template.

For particularly complex forms, I recommend creating a formGroup object as a separate file in each component's directory and then use `Object.assign()` to concatenate the parts into one whole.  This keeps the related code together while allowing you to assemble them into a form that Angular can use.

### onPush Notification

One of the optimizations available to Angular is a method of change detection called "Push Notification".  Simply stated, with Push Notification enabled, change detection for the component is only initiated if new data has been pushed into it via one of it's @Input() variables. Otherwise it is skipped.

In a large application, the performance gains using this mechanism are enormous. This is particularly true of an application that has an extremely nested or repetitive set of components. Each instance will be checked with each event that would cause the change detection cycle to kick off.

Since we will be striving to keep our components small, our apps are even more susceptible to the problems associated with change detection.

While it is true that Push Notification handles most of the situations, you will find that occasionally data in your component changes for some other reason. For this, you should use the ChangeDetectorRef methods.

### Styling

In an ideal world, you would have a theme that is external to all of your projects and that can be `npm install`ed into all the projects you work on.  This should control the basic look and feel of your application including:

- font color, size, family etc
- background color
- default style for common components

Lacking a separate project for your theming information, place this information in a global styles.css file.

Under no circumstance would you ever place the above information directly in a component file within your application.

Also, because all the main information is going to be in your theme, there is no benefit to using SCSS within your project. And, in fact, every project I've ever been a part of that tried to use SCSS as part of the project has become more of a mess and harder to maintain because of it.

Use SCSS for your theme project and CSS for your application projects.

## NgRX

We've already addressed how NgRX applied when using features and feature modules instead of one great big Store blob.

But, there are some other major places where NgRX gets misused that need to be addressed.

### One to One vs One to Many

If you are new to NgRX, one of the first things that will seem like a really good idea is that you should be able to create an action and have multiple Reducers or Effects respond to it. If you were to act on this impulse, you would (eventually) find that this ultimately makes your code hard to maintain because you are now calling multiple functions "at the same time."

From a maintenance perspective, this is a problem.  Imagine trying to track down the flow of execution in your application only to find that when you get to point X you now have to trace the action down multiple paths, not really knowing which ultimately is executing the code you are really interested in.

But, you have a further hidden problem. The order those functions get called is, for all practical purposes, undefined. At least, from a programming perspective, they should be considered undefined. The order is probably deterministic for any particular version of NgRX that you are running, but when you upgrade there is no reason to believe the code will still execute in the same order.

Similarly, your actions, reducers, and effects should only relate to each other. You shouldn't have an action that is part of slice A being handled by slice B even if it is a one-to-one relationship.

Some code I've seen has also aliased actions so that, technically action A is actually action B. Don't do that! All the above make your code incredibly hard to track flow of control.

If you were to do this, which I still don't recommend, you should create actions that are clearly multi-use actions.

So what if indeed a particular event needs to kick-off or update seperate slices of a store? The can be answered in a few ways.

* It may be worthwhile re-examining the architecture of your overall store. Is there good rational a single effect will affect to separate slices of a store in the first place? This is espcially important if the resulting actions end up doing the same thing or are using the same data. If so, consider normalizing the store slices and removing redundancies.

* If that passes the sniff test, consider dispatching different actions in sequence for that event. For example you may be updating different parts of the application each with
  different information and structure and or different service calls. Under this scenario, separate store slices will be updated via different store action sets and different information structures, regardless of whether they we initiated by the same event.

To reiterate, do not mix the various store slice actions just to intercept the same event.

### Flat Store

Another temptation you may encounter when you start working with NgRX is that you'll return data from the server in a nested format and then try to deal with it in your reducer in that form.

Don't do that!

Instead, flatten the data into multiple slices and use the Selectors to reassemble the data when you need it.

If you are going to flatten the data on the client side, you should use [Normalizr](https://www.npmjs.com/package/normalizr). Or, you could use [NgRX-Normalizr](https://www.npmjs.com/package/normalizr) and let it do some of the work for you.

If you have the option, you should flatten your data on the server before you return it. The main advantage to doing this is that you will return less data.

The reason you want to work with a flat store is because of immutability. Because the store is immutable, or at least, it SHOULD be, you will need to ensure that when you change an element of the data the object pointers above it all change as well. If you don't, your change detection mechanisms won't work correctly.

If you use Normalizr and reassemble the nesting in your selectors, you won't have to deal with this mess.

### Store Everything

You also may be tempted to only store some of your application state in the NgRX store.  Maybe you think storing search form data is overkill. That you'll pass the data over as it is needed. That might work, for a while.

But think about this. Once you've placed the data in the store, when you come back to a particular route, the data is still there. If you don't, the data is gone and you have to fill in the form again.

You might say, "But that's what I want them to do." Yes, but, what if the customer changes their mind? Now you have options.

Another advantage to the "Store Everything" approach is that you've pushed the logic for handling the data changes further down the stack. One place this becomes noticeable is that when you decide to process the data, you no longer have to pass the data with the Action to process the data. The Effect that processes the Action can now retrieve the data from the store.

If nothing else, this makes the code easier to test.  Code that is easier to test is code that is easier to maintain, even if you never write any test for the code.

### Data Transformation

There are two places where data transformations might need to occur.  The first is after we retrieve the data or right before we send the data back. In an ideal world the server would always send back exactly what we need in the form we need it and we would send it back in a similar form. There are at least two advantages to this.  The first is that it will reduce the amount of processing that the client side code has to perform. Second, the data that that comes back will be generally smaller.

The second place is as we are working with the data on the client side.

One scenario where this would occur is when you update a field and the side effect is that data someplace else on the screen should change as well. Not because you retrieved data from somewhere but simply because you changed data in an input field.

As I've already mentioned, we want to keep processing of data out of our components or even our component services unless the processing is specific to what the component does. The next logical place to put the processing of our store data is in our reducers.  But this has problems too. Maybe the data you want to display has no resemblance to the data that is in your store. Beyond that, this gets difficult to manage.

Instead, the best place to handle data transformation is in your selector. Selectors have a function call as their last parameter that takes all the data slices from the previous parameters. This function can manipulate the data and return it in any way that makes sense for the application. It has the added advantage of being able to leverage memoization and changing the object pointer as I've already mentioned.

### Services

A final temptation may be to ditch the Services that the Effects use and go after the REST end points directly. This seems to make sense since the Effects and the Services are both `@Injectable()`s but the problem with this is primarily that it violates the Single Responsibility Principle.

For retrieval of data, the Service is responsible for retrieving the data and possibly morphing it into the shape that we want it in. The Effect is primarily responsible for moving the resulting data into the store.

The reverse is also true. Effects are responsible for collecting the data from wherever it is in the store and passing it to the Service.  The Service is responsible for sending that data to the server in the form that it needs it.

One error I've seen is to inject the Store into the Service. The only `@Injectable()` that should be injected into your HTTP Service is HttpClient.

## Angular as Functional/Reactive

At the beginning of this article, I said one of the guiding principles of how we code is that we prefer Functional/Reactive programming over Imperative/Object Oriented programming.

What does that mean?

Imperative programming is code most of us are familiar with. It is code that says, do this, then do that, then do something else. Everything happens in sequence and it is very clear what happens when because the code says so.

The problem with imperative programming is that it often contributes to tightly coupled code. In fact, this is the main reason Object Oriented code uses dependency injection and inversion of control.

Object Oriented programming adds a layer on to imperative programming and tries to model everything in the world as a thing.  For example, a database Table, and User Interfaces are things. They lend themselves well to Object Oriented programming. By layering in dependency injection they decouple a lot of the code.

But Object Oriented code still suffers from a problem. Not everything we are trying to write code for is a thing. Most of what we code is a process. Until recently, we've tried to shoehorn in a programming model that works for things and tried to make processes things somehow. It hasn't worked well.

Another problem with Imperative/Object Oriented programming is that it mutates memory and each function can return different values even though I've passed in the same values. This can make this model difficult to test.

Think about it, which would you rather write a test for?  Code that given the same parameters always returns the same value, or code that is indeterminate.

But wait, you say, all code is deterministic. Or is it?

Take for example a method in a class that has two member variables.  The method itself takes a parameter and makes a call out to a database. How many possible return values are there for any given value we pass in?

Well, here's the problem, you actually have 4 parameters and one of these is difficult to control. Oh, sure, you could mock the call to the database and get some control. But if you could write code that didn't require taming, wouldn't that be helpful?

Which leads us to Functional Programming. In Functional Programming we have a number of key advantages. First any given function passed the same parameters always returns the same value. Testing issues solved! Second, strictly functional code has no variables. As one article I read states, "You can't screw up what you can't change." Third, everything is a function.

This is an oversimplification, you can read more on functional programming elsewhere. One final advantage that most of the literature doesn't mention is that Functional Programming models processes better than Object Oriented programming. And because of the features I mentioned above, it is also much easier to test and much more deterministic.

Which leads to Reactive. Aside from the fact that we are using RxJS in Angular which is a Reactive library, this also means that we react to "events" that get fired.  This is essentially what is happening when you dispatch an action in NgRX, what you are effectively saying is, "someone do this thing for me."

In the past, you would have just coded for that process to start. Here, that action could just as easily happen on some other computer, or at least some other thread. And so, instead of coding "do this, then that, then this other thing."  You are coding, "when I get notified, I'm going to do this and then, optionally, notify the system that I'm done."

And so, we favor Functional/Reactive programming because it makes our programs more stable but we admit that not everything can be done using a Functional/Reactive model.
