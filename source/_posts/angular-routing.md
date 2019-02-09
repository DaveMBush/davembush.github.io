---
title: How to Implement Angular Routing
tags:
  - angular
  - routing
url: 4388.html
id: 4388
categories:
  - Angular
date: 2017-07-18 06:30:23
---

In the old world where all of our pages were on the server and every change on the client side required a full round trip to the server, each page was a unique URL on the server.  In the SPA world, we only load one “Page” from the server and the client takes care of making it look like we have moved from one page to another. 

When done well, we can create pages that reuse existing content on the screen causing a minimal screen refresh while still allowing the user to link to a specific “Page” in our application. 

These “Pages” are called “Routes”  As in, here is the route to some code I want to execute. <figure>![](/uploads/2017/07/2017-07-18.png "How to Implement Angular 2+ Routing") Photo via [Visual Hunt](//visualhunt.com/re/9589c4)</figure>

<!-- more -->  Sounds pretty cool.  But there is a down side that shows up every time someone tries to do this for the first time.  You won’t see this problem until the first time you try to deploy your code because the development server handles this issue for you. 

The problem is this.  When a server receives a request from the browser, it tries to find that file on the server.  If it doesn’t exist, the server returns a 404 error.  File not found. 

Most servers provide ways of circumventing this issue by providing rules.  Essentially, you write a server rule that says, “If the browser ask for a file that doesn’t exist, send them back index.html instead.”  You may need to provide exceptions or otherwise refine the rule if your server is also rerouting other traffic. If you are running on an IIS server, [I wrote instructions for how to take care of this issue](/asp-net-angular-js-html5mode/) for AngularJS (1.x).  The instructions work for any client side framework that implements routing. 

Assuming you have that end of things working correctly, here are the steps to get basic routing working in your Angular application.

Define Your Routes
------------------

While we could easily define our routes in app.module.ts, the code we write will be much easier to maintain if we create a separate module file named app-routes.module.ts.  So to start, create an app-routes.module.ts file right next to your app.module.ts file.  You can do this with the Angular CLI by typing the following in the command line from within the src/app directory: 

``` shell
ng g module app-routes
```

When you create a module with the Angular CLI, it will put it in a sub-directory.  In this case, it created an app-routes sub-directory.  We want it next to our app.module.ts file.  So, now we need to move the module up a directory and remove the app-routes directory. 

Open up the file, it has some stuff in it that we don’t need.  Remove the CommonModule references and the declarations section of the @NgModule decorator. 

In this new file, you will create an empty Routes array, called routes and decorate the class with @NgModule 

``` typescript
export const routes: Routes = [];

@NgModule({})
export class AppRoutesModule {}
```

You need to also import Routes and while you are doing that, you might as well import RouterModule because you are going to need that soon too. 

``` typescript
import {RouterModule, Routes} from ‘@anguler/router’; 

export const routes: Routes = [];

@NgModule({})
export class AppRoutesModule {}
```

Next, in your app.modules.ts file, import AppRoutesModule using both the TypeScript import and as part of the imports section of the @NgModule decorator.

``` typescript
import {AppRoutesModule} from ‘./app-routes.module’;
@NgModule({
  …,
  imports: [
  …,
  AppRoutesModule
  ],
  …
}
```

We really haven’t done anything useful yet, we’ve just setup some boilerplate code that will compile so we won’t have to think about it any more. 

Now, back to the app-routes.module.ts file. 

Each element in our Routes array defines a specific route in our system relative to the parent route it is a part of.  At the top level the parent route would be the root of the application. 

Here are the properties that are available to us:

### path

The path property allows us to specify what path, or URL, will load this route.  If you want the component to load for any path, use ‘**’ as the value.  If you want the component to load for the root element, use ‘’ for the path and specify pathMatch: ‘full’ as another property.  You can also use the value ‘**’ to mean, “match anything.”  We typically use ** to match what would typically be thought of as 404 errors.  For this to work correctly, it should be the last element in your top most route definition.

### pathMatch

As we’ve already mentioned, pathMatch should be ‘full’ to match ‘’ as the exact path.  But you can also give this value ‘prefix’ to tell it to match any path that starts with the value.  You only need to specify this value if you want to use ‘full’. 

It should also be noted that this value only evaluates the part of the path you are in.  If you use this in a child path, it won’t match the whole path, but only the part that is in the child.

### component

Component specifies what component should get loaded when the path is matched.

### children

The children property allows us to specify an array of child paths.

Route Components
----------------

Since our routes will need components, let’s start by creating several components so that we can illustrate routing. 

But first, a short word about how we organize our code. 

In many demos online, the tendency is to put all of our components right under the app directory.  But, in larger applications, I’ve found that it makes a lot more sense to create a route directory under the app directory that we place each of our routes in. 

Now, you might think that we would want to place our child routes as child directories under the routes they are a part of, but the problem with this is that we often have child components in our routes.  How do we know which directory represents a child route and which represents a child component? 

No.  

What we really want to do is place even the components that represent child routes right under our routes directory.  So, say we have a Page1 route and there is a SubPage route that is a child of Page1.  To make it clear, we put SubPage in a directory named page1.sub-page. 

As for components that are common to multiple pages, we place those in a components directory which is right under the app directory.  This keeps our directories well organized and the code neatly organized as well. 

The next obvious thing that we need to do is that we need to create a routes directory.  Do that now. 

Now, at the command line, inside the new routes directory, execute the following Angular CLI commands 

``` shell
ng g component page1
ng g component page2
ng g component page1-subpage
```

As you executed each command, it should have created a directory for each component with the corresponding css, html, ts and spec files.  Then it updated the app.module.ts file for you so that the components are available for use in the system. 

You may also notice that we created the component as page1-subpage instead of page1.subpage.  The reason for this is that the CLI doesn’t like period separation of file names.  Now, the next thing we are going to do is change the directory name to page1.subpage.  You will also need to change the TypeScript import line that references this directory in your app.module.ts file. 

Now that we have components to page to, let’s create our route definitions.  Back to the Routes array in our app-routes.module.ts file. 

``` typescript
export const routes: Routes = [{
  path: ‘page1’,
  children: [
  {
    path: ‘’,
    pathMatch: ‘full’,
    component: ‘Page1Component’ },
  {
    path: ‘subpage’,
    component: Page1SubpageComponent
  }]}, {
  path: ‘page2’,
  component: Page2Component
}]
```

The first definition may look a bit odd.  We are setting up a route to page1, but the route is the children.  Then in the children we define a route to ''.  This is where the Page1Component is specified as the component we want to load. 

You will also note that we specified `pathMatch: 'full'` for the Page1Component.  This is because we only want this component to be loaded when the child path is empty. 

Using this definition, everything loads into the top level router-outlet.  If we placed Page1Component at the same level as we defined the page1 path, then Angular would expect to have a router-outlet in Page1Component where Pag1SubpageComponent would be loaded. 

Needless to say, you need to be careful how you define your routes. 

Next, you will need to import the three components using the TypeScript import statement. 

``` typescript
import {Page1Component} from './routes/page1/page1.component';
import {Page2Component} from './routes/page2/page2.component';
import {Page1SubpageComponent}
  from './routes/page1.subpage/page1-subpage.component';
```

Now that everything is defined, we just need to tell Angular where we want these components to show up.  For right now, open the app.component.html file and remove everything that is there and add the router-outlet component. 

``` html
<router-outlet></router-outlet>
```

Now, `router-outlet` is a component that is defined in the RouterModule, so we need to import that in the imports section of our AppRoutesModule.  But we don’t just import the RouterModule, we use RouterModule.forRoot() and pass in the route array we just defined into forRoot(). 

``` typescript
@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutesModule {
}
```

There is one final tweak that we need to make to our route definition.  Right now, if you go to the root of the application, there isn’t a component defined for it.  To fix this, we are going to add the following definition at the top of  our routes: 

``` json
{
  path: '',
  redirectTo: 'page1',
  pathMatch: 'full'
},
```

You need `pathMatch: 'full'` to tell the router to only match this rule when the path is '' exactly, just like we did with the Page1Component in the children list.  Otherwise, it will match everything. 

The `redirectTo: 'page1'` part tells it to redirect to the page1 path when this rule is true.

Lazy Loading
------------

I realize that I still need to show you how to navigate to routes in your application, but first let’s look at lazy loading the routes. 

In the past, when building a Single Page Application, the custom was to load all of the JavaScript code we needed for the page up front.  But, if you have multiple pages in your application, some of those may never be needed by your user.  So, what are we doing loading stuff that will never get used? 

No, it is better to load only what we need when we need it.  While it might take longer if you totaled up each load, the user perceives the experience as faster.  Therefore what we want to do is to make each of our pages load as we need them. 

To do this, we need to create a module for each of the components that represent our top level routes.  We want to import modules and services into the module that is closest to where we need them.  This is why.  By only importing things where we need them, Angular can create the smallest package necessary all up and down the dependency tree. 

To make things easy and to do them the way you would have done them if you had done it this way to begin with, let’s delete all of the sub-directories under routes.  It’s OK.  We aren’t losing any work that we can’t quickly get back.  We haven’t added any code to these routes yet. 

At the command line, navigate to the routes directory and then type in the following Angular-CLI commands:

``` shell
ng g module page1
ng g component page1
ng g module page1-subpage
ng g component page1-subpage
ng g module page2
ng g component page2
```

And then, just like we did the first time, rename the page1-subpage directory to page1.subpage. 

Now, go to the app.modules.ts file and remove the references to the Page1, Page2, and Page1Subpage components anywhere you see them. 

Do the same thing in the app-routes.module.ts file. 

Now, the way we define our routes changes slightly.  We will still need the redirect route, but everything else changes. 

The key to making this work is the property `loadChildren`, which is a string in the format of: 

``` typescript
'pathToModule#ModuleClassName'
```

We’ll work from the top down.  Change the routes array in app-routes.module.ts to look like this: 

``` typescript
export const routes: Routes = [{
  path: '',
  redirectTo: 'page1',
  pathMatch: 'full'
}, {
  path: 'page1',
  loadChildren: './routes/page1/page1.module#Page1Module'
}, {
  path: 'page2',
  loadChildren: './routes/page2/page2.module#Page2Module'
}]
```

When we try to access something from page1, it will load the Page1Module and try to resolve it from there.  When we try to access something from page2, it will load the Page2Module.  Both of these happen during run time. 

Next, go to page1.module.ts and, import RouterModule and add the following to the imports section of @NgModule.

``` typescript
RouterModule.forChild([{
  path: '',
  pathMatch: 'full',
  component: Page1Component
}, {
  path: 'subpage',
  loadChildren: ‘../page1.subpage/page1-subpage.module#Page1SubpageModule’
}]),
```

Next move over to page2 and do something similar.  Since page2 doesn’t have a sub-route, you only need, one route. 

``` typescript
RouterModule.forChild([{
  path: '',
  pathMatch: 'full',
  component: Page2Component
}]),
```

And again, similarly for page1-subpage. 

``` typescript
RouterModule.forChild([{
  path: '',
  pathMatch: 'full',
  component: Page1SubpageComponent
}]),
```

If you haven’t already, move your command-line prompt back to the root of the project and type

``` shell
npm start
```

To start the server and compile your code.  If everything compiles, you should see 3 chunk files along with the other files we saw when we compiled the code without lazy loading. One each for each of the routes. 

Run the app in your browser now to make sure it works correctly. 

See how easy that was?  It isn’t really that much harder than specifying the routes like we did the first time, but we get huge benefits in performance.

Passing Parameters
------------------

The last thing you need to know about is how to pass parameters.  You would normally do this when you are coming from an existing list of items.  Each item has some sort of unique identifier.  We click some link and that takes us to another page to show details or to edit the content.  For our purposes here, it doesn’t matter. 

To specify that a route takes a parameter, use colon notation: 

``` typescript
path: 'detail/:id' 
```

Angular knows it is a parameter when you use a URL to get to it because of the location.

Retrieving Parameters
---------------------

Let’s say you have a component that represents a route with a parameter.  For that to be useful, you’ll need to pull the parameter out of the route information. 

To do this, you’ll need to inject `ActivatedRoute` into the component.  Then when you need the parameter(s) you can use: 

``` typescript
route.params.take(1).subscribe(
  params =>
  // Use params['id'], where ‘id’ is the name
  // we gave the parameter in the path.
  );
```

Route Navigation
----------------

Now that we have routes in place, we need to discuss how to navigate from one route to another.  The temptation, having just used URLs to go from one to the other, would be to use hyperlinks and put the information in the href attribute. 

No doubt, you could probably get that to work, but the main problem with using that method is that there is no safeguards to make sure that the URL you use to navigate when you are developing will work when you move the site to another environment. 

The reason for this is that we have to set the base href for the site.  During development this is normally ‘/’.  But when you go to production, it could be some sub directory. 

Also, because of this base href, every page/route we land on is still relative to that base.  This means that every route we want to navigate to would have to be hard wired to the base of the site, and again, that’s assuming that the site will always be in the same relative location when it is deployed. 

Now, if we can’t using a regular URL to navigate, what do we use instead?

### routerLink

You use the routeLink directive added to your anchor tag.

``` html
<a [routerLink]="['/page1']">go here</a>
```

This may look a little different from what you expected, so let’s break this down. 

The routerLink directive takes an array.  Since we can’t pass an array as a string, the only way we can pass it is by evaluating it at run time.  Remember, the square bracket syntax is an indication to the Angular compiler that what we are assigning is something that should be evaluated.  Typically this would be pointing to a function or variable in our TypeScript code.  In this case, we are pointing to a literal array.  Everything between the opening and closing quotes is JavaScript. 

As for the actual parameter, the string in the single element array works much like you would use a URL.  The forward slash says to start at the root of the web application (instead of the root of the domain like a URL would.)  And the page1 is the route we’ve already defined.  If you leave the forward slash off, it is relative to the current route. 

But what about passing parameters? 

``` html
<a [routerLink]="['/page1', someVariable]">go here</a>
```

Each comma delimited value represents a segment of your route.

### Router.navigate()

The other way you might want to cause navigation to a page to occur is by using the navigate() method hanging off the Router class.  Using dependency injection, you inject the Router into the class that needs to use it and then use that instance to call navigate().  The parameter you pass in looks very similar to what you used for routerLink. 

``` typescript
router.navigate(‘/page1’,someVariable);
```

Yes, both routerLink and Router.navigate() both support URL like references using './path' or '../path'.

Where Am I Now?
---------------

The last part of routing you will commonly need to know about is detecting what the current route is.  Once again this will require you to inject the Router object into the component that needs the information.  Once you have the router object, you can use code like this: 

``` typescript
router.routerState.snapshot.url;
```

This will get the current route url.  I normally grap this as part of listening for the router’s NavigationEnd. 

``` typescript
this.router.events
  .filter(arg => arg instanceof NavigationEnd)
  .map(arg => {
    this.selectedTab = router.routerState.snapshot.url.split('/')[1];
    return this.selectedTab;
  }).subscribe();
```

Guards
------

Guards control access to our routes.  What happens if you have a route that only certain people should have access to.  Like an admin page.  Sure, you could leave the link off so no one can click the link to get to the page, but that doesn’t prevent someone from pasting the link to the forbidden page into the address bar of the browser and getting there anyhow. 

In Angular, we have four kinds of guards and two ways of creating them. 

The four types of guards are:

* CanActivate
* CanActivateChild
* CanDeactivate
* CanLoad

If you follow my advice and always lazy load your routes, than the two you will most often use are CanLoad and CanDeactivate.  CanLoad provides rules for lazy loading a module.  CanDeactive provides rules for leaving a route. 

If you decide to bundle routes together, then you may also need CanActivate and CanActivateChild.  CanActivate is exactly what it sounds like.  Can I activate this route?  CanActivateChild would go on a route definition that has a children’s collection.  This rule determines if I can activate the children. 

To use Guards in our application, the first thing we need to do is to define them.  The easiest way to define them is as a function that returns a boolean value, a boolean Observable, or a boolean Promise.  For our purposes here, we will just return a boolean value.  But when you have some asynchronous call you need to make to determine if we should return true or false, you’ll want to return an Observable or a Promise.  I favor Observables. 

The definition for a Guard rule looks like this: 

``` typescript
@NgModule({
  providers: [{
    provide: 'ruleNameHere',
    useValue: () => {
      return true;
    }
  }]
})
```

Then, to use the rule, you assign the appropriate rule the name of the rule. 

``` typescript
{
  path: 'page1',
  canLoad: ['ruleNameHere'],
  loadChildren: './routes/page1/page1.module#Page1Module'
}
```

Notice that canLoad, as well as the other guard properties, takes an array.  This allows you to apply multiple rules to a route. 

The other way of defining a route is as a class that implements an interface, or multiple interfaces that include CanActivate, CanActivateChild, CanDeactivate, and CanLoad.  You them implement the corresponding functions in your class. 

Now, to include the rule you use the Class rather than a string: 

``` typescript
{
  path: 'page1',
  canLoad: [RuleClassHere],
  loadChildren: './routes/page1/page1.module#Page1Module'
}
```

But wait, there's more ...
--------------------------

Believe it or not, there is even more to routing than we've discussed here.  But we'll leave that for another day or this post will turn into a [book](https://davembush.github.io/get-started-with-angular/). 