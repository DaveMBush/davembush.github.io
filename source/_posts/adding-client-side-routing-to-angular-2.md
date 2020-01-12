---
title: Adding Client Side Routing to Angular 2
tags:
  - angular
  - routing
url: 4104.html
id: 4104
categories:
  - Angular
date: 2016-11-29 07:30:00
---

Over the last several Angular 2 posts, we’ve been building up our application bit by bit and examining the various features of Angular 2 along the way using the Angular CLI where that makes sense and modifying it along the way. So far, routing is an area that the Angular CLI does not yet support and so, when you want to use routing in your Angular 2 application, you’ll need to wire most of it in by hand. Now, the routing engine has changed several times during the development of Angular 2. And I know you’re wondering which version of the router this article is going to be talking about. So, to be clear, this article was written using the Angular CLI version 1.0.0-beta.21 and Router version 3.2.1.

<figure>![](/uploads/2016/11/image-4.png "Adding Client Side Routing to Angular 2")<figcaption>Photo credit: [xomiele](//www.flickr.com/photos/xomiele/6759264721/) via [Visualhunt](//visualhunt.com/photos/snow/) / [CC BY](//creativecommons.org/licenses/by/2.0/)</figcaption></figure>

<!-- more -->

What is Routing?
----------------

If you are new to developing Single Page Application (SPAs) you might wonder what Routing is. You might be surprised to find out that you already know what routing is, but you’ve never actually called it by this name.

For example, using MVC in the ASP.NET world, you used routing whenever you specified what controller you expected your code to hit when you specified a particular URL. You’ll remember that we set our code up so that when we specified that URL, code in a controller would get hit. We also had the option of specifying parameters that we wanted to have passed into our controller methods.

In a SPA, routing does essentially the same thing. The only difference is that we never have to call the server. This makes all our “Pages” virtual. Instead of requiring that our visitors always start at our home page and navigate into the rest of our site; instead of creating a separate page on the server for each page in our site; we can load all the site up front and the user can navigate to exactly the page they want to be at. They can even link directly to that page and the client side will handle displaying the page appropriately.

There’s a Catch
---------------

One of the problems you’ll quickly discover is that for this to work, you’ll need to set your server up so that it doesn’t try to handle the routing as well.

You see, if you navigate to the home page of your web site and then click around into the sub-pages, everything is going to appear to work correctly. But once you try to navigate directly to an inner page, you are going to become extremely frustrated. The problem lies in order your code gets executed.

When you request a page directly, what happens is that the server will look for that page on the server. If it can’t find it, it will return, appropriately, a 404 error. The problem is that when we ask for a page that only exist because the client side has said it does, when the server goes to look for it, it will return the 404 error. It isn’t there.

For the moment, I’m ignoring the fact that Angular 2 supplies a feature called Server Side Rendering, which can also take care of this problem. Given regular, out of the box, Angular 2 code, you’ll want to make sure you server knows what to do when the files don’t exist. What I normally do is that I create a rule on my server that says, “if I’m looking for a path that doesn’t have an extension, just return the index.html page you would have returned if I had asked for the home page.” In Express on Node.JS, the code for this looks like this:

``` javascript
// This comes last.  right before we start listening
app.use(function(req,res){
    // this sends back the index.html file when
    // it looks like they are looking for
    // a client side route
    // assuming a real file will have an extension
    // and a route will not.
    if(req.url.indexOf('.')\> -1){
        res.status(404)        // HTTP status 404: Not Found
            .send('Not found');
    }
    else {
        res.sendFile(__dirname + '/www/index.html');
    }
});
```

If you are using IIS and ASP.NET, you might find this article I wrote about using [Angular, Routing, and ASP.NET](/asp-net-angular-js-html5mode/) useful.

Enabling Routing
----------------

Since the Angular CLI has included the packages you’ll need to enable routing, there is nothing to install. We just need to write some code.

Typically routes get enabled at the top of your application after all the common code has been implemented. So, in the location where you want the routing to take effect, add the following tag:

<router-outlet></router-outlet>

I’ve added this to my `app.component.html` file in the sample app I’ve been working on. Replacing the `<h1>{ {title}}</h1>` code that we had from the previous weeks.

If you were to run the code now all that you would see is that the title no longer shows up. We need to add the route code next. You will notice that an `app-routing.module.ts` file already exist. Open this file. You will see that the bulk of the code we are going to need is already there.

You should see a line that looks like:

``` typescript
const routes: Routes = [];
```

We are going to add a route to this array:

``` typescript
export const routes: Routes = [
        {
            path: '',
            component: ViewComponent,
        }
];
```

What this is saying is that whenever we ask for the home page, load the “ViewComponent” component.

But wait, we haven’t added any components yet.

While we COULD just add a component, what I prefer to do is to add a new module. This is because I dislike the idea of making all the components in my code part of one huge modules. It makes it extremely difficult to refactor my code. I also want the ability to implement lazy loading in the future and I will need the component I am routing to, to be part of its own component in order for that to work.

To add a View component to our code run the following line from the terminal/command line `ng g module view` This will create a new ViewModule module with a new ViewComponent component located in the view directory.

The rest of what we need to do is to just wire this all into the existing application.

Go back to `app-routing.module.ts` and add an import statement to load in `ViewComponent` and the `ViewModule`:

``` typescript
import { ViewComponent } from './view/view.component';
import {ViewModule} from "./view/view.module";
```

We also need to add `ViewModule` to the imports array.

``` typescript
@NgModule({
  imports: [
    RouterModule.forRoot(routes),
    ViewModule
  ],
  exports: [RouterModule],
  providers: []
})
```

Next, we need to register the routing module with the application. So, load up the app.module.ts file and add an import statement to import the app-routing module:

``` typescript
import { _applicationName_RoutingModule} from './app-routing.module';
```

And add `_applicationName_RoutingModule` to the imports array:

``` typescript
@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    FormsModule,
    HttpModule,
    _applicationName_RoutingModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
```

Adding additional routes is more of the same. Let’s add another module for editing.

`ng g module edit`

Add two new paths. One so we can add a new item and one so we can edit an existing item.

``` typescript
const routes: Routes = [
  { path: '',
    component: ViewComponent
  },
  { path: 'Add',
    component: EditComponent
  },
  { path: 'Edit/:id',
    component: EditComponent
  }
];
```

Notice that for the Edit command I added :id at the end. The :id specifies that this location is where the parameter will be. In this case, the ID of the record that we want to edit.

We will flesh these out later and I’ll leave adding import statements and adding modules to the import arrays to you. It is essentially copy/paste/modify from the previous code. When you have the code working, you should be able to navigate to /Add or /Edit/id and see the new page.

If you get stuck, the code so far can be found here: [https://github.com/DaveMBush/GettingStartedWithAngular2/tree/Step3](//github.com/DaveMBush/GettingStartedWithAngular2/tree/Step3 "https://github.com/DaveMBush/GettingStartedWithAngular2/tree/Step3")
