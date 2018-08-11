---
title: Jedi Angular 2 Tips and Tricks
tags:
  - angular
  - css
  - tips
  - tricks
  - typescript
url: 4246.html
id: 4246
categories:
  - Angular
date: 2017-03-28 06:30:00
---

This week, I thought I’d collect a list of unrelated tricks and tips I’ve learned over the last couple of weeks into one post.  Unless you love to read documentation, or you’ve run into problems that these tips solve, I’m guessing you don’t know most of these. But first, the big new this week… <figure>![](/uploads/2017/03/image-4.png "Jedi Angular 2 Tips and Tricks")<figcaption>Photo credit: [iamkory](//www.flickr.com/photos/korymatthew/14211839966/) via [Visual Hunt](//visualhunt.com/re/4d4175) / [ CC BY](//creativecommons.org/licenses/by/2.0/)</figcaption></figure>

<!-- more --> 

Angular Version 4 Released
--------------------------

You may have already seen this.  You can read all about it [on their blog](//angularjs.blogspot.com/2017/03/angular-400-now-available.html).  Yes, it makes the code smaller and faster.  Yes, it implements an “else” for *ngIf.  Yes, it works with the latest version of TypeScript.  But the one change I’m most looking forward to is the “source maps for templates.” If you’ve ever seen the error in your console window indicating an issue with your template with some obscure line number that has nothing to do with your template code, this should be great news for you as well.  Imagine being able to actually track down those errors in some more intelligent way than trying to guess exactly what line is causing the problem. There is one small breaking change in this release and that is that they’ve moved the Animations package out of Core and into its own location.  Since we’ve decided to wait to hear from our control vendor to see if that is going to have any impact, I can’t report on how much heart burn that’s going to cause you.

The Official Name of Angular
----------------------------

So, finally the Angular team has come up with an official way of distinguishing between Angular 1 and everything after it.  The official name of Angular 1 is “AngularJS.”  Everything else is just “Angular.”  For now, I’ll keep referring to it as Angular 2 until it actually catches on.

Periodic Production Compiles
----------------------------

For the purposes of this article, I’m going to assume you are using the CLI, but if you are doing all of that work by hand, most of what I have to say will translate to what you are doing. A couple of weeks ago, we ran into a memory leak in our code that evidently is a known Angular 2 issue and only appears when you compile for DEV mode.  This caused us to go down the path of actually trying to compile our code in Production mode where I found that some of my code wouldn’t compile. As it turns out, when you compile in production mode it verifies that code more strictly so that it can use Ahead of Time compile if you choose to do that. Moral of the story is that you probably want to always work in production mode until you have an issue that you want to track down, where you can switch to DEV mode for the debug session.  Or, at the very least, compile for production prior to running the code in debug mode just so you are sure you can compile.  I prefer always running in production mode so that I’m sure the code is working correctly. Before you get all, “it should work the same way in both environments” on me.  Stop and think about this for a few seconds.  This is JavaScript we are talking about here.  A dynamic language.  You can’t have things like tree shaking and ahead of time compile AND get all of the benefits of a dynamic language at the same time.  I’m sure if it were possible, it would have been built to work closer to the same way to start with.  Not that I don’t agree with you, but unless you know how to fix it and have issued a pull request to address this issue, you have very little room to complain.

Direct Access to CSS
--------------------

When I wrote about how to [add CSS to an Angular project](/adding-css-and-javascript-to-an-angular-2-cli-project/), someone commented asking how to see the raw CSS instead of the JavaScript that it turns into.  Well, as it turns out, there is a build flag to control that. `ng serve --extract-css` I personally never need this because my files are organized in such a way that I know where stuff is, but I can see where this would be helpful if I wanted to track down CSS in someone else’s code.

Accessing the Component Host Element
------------------------------------

### CSS

If you need to access the host element for your component using CSS, the selector you want to use is `:host.` When you use this, keep in mind that your element has no default style so you will need to provide all of the styling.  Otherwise, it will show up as an empty element with overflow set to show.  This is probably not what you want. By using `:host`, you can avoid creating an inner `DIV` in your component just so you can display everything else.  The host element can function as that `DIV`.

### TypeScript

You can access the host element directly from your typescript using `this.elementRef` You will need to inject `ElementRef` in your constructor for this to work.

### Events

Finally, if you need to bind to events on the host element, you can use the @HostEvent() attribute prior to the function that is going to handle the event `@HostListener('click',['$event']) foo(event) {` }

Accessing Child Components
--------------------------

### CSS

If you need to style a child component from one of your components in some way that is unique from the rest of the system, you will need to add CSS in your component.  The proper way to access the child component is by using this directive: `:host >>> child-component` Where, `child-component` is the tag you are using to load the child component. The trick is the triple > selector.

### TypeScript

To access the child component in your TypeScript, all you need to do is give the element you are interested in a variable using the #variable identifier.  Notice, this is standalone.  We are not using the ID of the element, although it could have the same name if you wanted. Then, in your typescript, you’ll declare a public variable that is prefixed with the `@ViewChild()` decorator.  There are several ways that you can use this.  But using the variable method that I’m describing here, you just specify the variable name, in quotes. `@ViewChild('variable') variable: ElementRef;` Everything is an `ElementRef`.  If you need to type it more strongly, you can. You can use whatever variable name you want to for the member variable.  But I normally name it the same as what I named the element.

Animations
----------

One of the cool things we can do with CSS is animate DOM elements using CSS.  If you haven’t discovered this yet, you want to because it avoids the JavaScript –> DOM overhead making it smoother and more energy efficient on handheld devices. The problem is, because the elements we want to animate in and out often don’t exist in the DOM at the right time, using straight CSS to do the animations is problematic or impossible.  This is where the Animations package comes in. On the project I’m working on now, I define my animations in an animations directory and apply them to my components as needed.  So, for example, I have an animation defined that fades my pages (routes) in and out.  It is defined once and applied to all my top level components.

Flex Layout
-----------

Another rather new CSS development is the flex-box layout system.  This solves all kinds of problems that we’ve had in the past.  The problem is, it is so new that it doesn’t work exactly the same way on all of the browsers. What the flex-box layout system does is that it allow us to layout rows and columns specifying the width of the columns or rows within them.  The Flex Layout package takes that concept and expands on it as well as making sure it works the same way across all browsers. First, instead of using CSS, it does all of the same work using directives.  This puts the layout closer to the code we want to impact without violating the single responsibility principle.  Think about it.  Layout stuff like this is generally component specific.  Not something we want to  generalize beyond that.  In the old days, we did need to generalize layout like this more because we were applying it to multiple pages.  But our common layout is typically handled by our top most component in a SPA. Beyond all of this, it also allows you to specify when a particular directive, or element, should apply based on screen size.  So, it takes the Bootstrap grid layout system and makes it even more powerful.  I’ll probably still use Boostrap for what a component looks like, but I’m planning to move the bulk of my page layout to Flex Layout.

Optimize Change Detection
-------------------------

If you are coming from the AngularJS world, you are use to Angular doing all of your change detection for you.  And by default, this works similarly in Angular.  But, you can improve the performance of your Angular application by using OnPush change detection.  This makes it so that the component will be marked for change detection when the pointers it is looking at change.  This way, it doesn’t have to (for example) evaluate all of the elements in an array just to figure out if it need to redraw the component. However, sometimes this doesn’t work.  In this case, you’ll need to tell the component that it should redraw.  To do this, make sure you’ve injected `ChangeDetectorRef`and then call the member function `markForChange()` to let the system know, this component should be updated. I don’t know about you, but I’m loving Angular!
