---
title: 'Angular 2 – First Impressions [Compared to Angular 1]'
tags:
  - angular
  - javascript
url: 3406.html
id: 3406
categories:
  - Angular
date: 2016-02-25 08:30:00
---

I’ve spent the last week putting together [a reference app for Angular 2](//github.com/DaveMBush/MEA2N_CRUD_Reference_App).  It is a great exercise to try to nail down the basics of how a new framework works.  Next week I plan on doing a similar exercise for [React](//facebook.github.io/react/).

Anyhow, I thought for this week’s post, I would try to relay some of my impressions, and some of the issues I see with this new framework.

![Angular 2 - First Impressions](/uploads/2016/02/image-3.png "Angular 2 - First Impressions")

Why call it Angular 2?
----------------------

I realize naming things is always hard, but I think naming something similar to something that already exist is dangerous.

When .NET first came out, many ASP programmers tried to use ASP.NET as though it were ASP.  That didn’t turn out so well for them and, I think, confused them.

VB.NET had a similar problem.  During those early days of .NET I was working for a training company.  I had several ex VB6 developers show up to my C# class.  Do you know why?  Because they realized that VB.NET was such an entirely different language, they were better off just starting over with a new language than possibly treating VB.NET as though it were VB6.  Good call if you ask me.

Most recently, Microsoft has learned from history and decided to rename the product they’ve been referring to as .NET 5 as .NET Core 1.0.  In this case it is enough like .NET that keeping .NET in the name is a good thing.  But starting the versioning at 1.0 again also shows that this is a brand new product with some differences from what we are all used to.

Rob Eisenburg decided to call his new project Aurelia instead of Durandal 2 because they are enough different from each other.  While I’m not an expert on either product, I’m pretty sure that there are elements of Durandal in Aurelia simply because Rob was significantly involved in both projects.

Which brings me to Angular 2.

Why call it Angular anything?!  Angular 2 has very little in common with Angular 1.x other than basic concepts.  But, so much has changed it is like calling “ASP.NET 1.0” “ASP 5”.  It just doesn’t work, and it hides the fact that there is going to be a HUGE learning curve for those who are familiar with Angular 1.

So What Has Changed?
--------------------

### Case sensitive vs Snake Case

In Angular 1, all of the directives that we create are case sensitive in our code and snake-case-in-our-html.  In Angular2, the HTML markup is case sensitive.  Rob went on a rant about this [here](//eisenbergeffect.bluespire.com/on-angular-2-and-html/).  His issue is that Angular 2 isn’t HTML spec compliant because it is case sensitive.  I’m really not sure how much this matters as long as the final result is compliant.  My issue is more along the lines of, “did this really need to change at all?”

### Basic Syntax Changes

Then there are the basic syntax changes.  If you want to bind to an html element property you use square brackets.  If you want to fire an event, you use parenthesis.  If you want two way databinding, you use both.

What? We didn’t need this distinction with Angular 1.  Why the distinction in Angular 2?  I suppose it has something to do with making it easier for Angular 2 to parse the HTML.  But, from the outside looking in, the thing you are binding to should make it clear enough what it is you are trying to do.  It reminds me of Pascal (the language).  More syntax for the sake of making sure the programmer can’t shoot himself in the foot.

### TypeScript

I fell in love with TypeScript back at version 0.9.  The advantage to an object oriented programmer is that you don’t have to think differently about your client side code using TypeScript than you would about your server side code if you are using a real object oriented language like C#, or Java on the server side.

But TypeScript isn’t JavaScript.  And while you can write Angular 2 using JavaScript, the preferred language, and the language that the bulk of Angular 2 is written in, is TypeScript.

To confuse matters, rather than tell the world, “Hey, just use TypeScript” they decided to give us options.  You can use Dart (what’s that?) JavaScript, or TypeScript.  We’ve already been through this with VB.NET and C#.  Doesn’t anyone learn from history?  Eventually C# won.  Oh, there are a few hold outs.  But even most of the VB lovers have given up and moved to C#.  It will be interesting to see how this TypeScript move plays out.

On the one hand, I think TypeScript is a better version of JavaScript.  On the other hand, if all you know is TypeScript, it will make it harder to find a job as a JavaScript programmer.

While TypeScript is definitely a better choice than having a new proprietary language just for Angular, which was the original plan, I still think we might have better off using pure JavaScript as the language of choice.

Like I said, I’m a fan of TypeScript, I’m just not sure making this the default language is a good choice.

### Everything Visual Is a Component

Unlike Angular 1 where you have pages and directives, everything in Angular 2 from a visual perspective maps to an element.  This is a good change.

At first when I heard this, I had trouble having it make sense in my mind.  But having implemented it once, I see that it really does make a lot of sense.

You can also create new attributes that you can attach to an element.  But attributes add behavior to an element rather that making an element do or be something entirely different like you could do with Angular 1.

### Component CSS

I'm really not sure if I like this feature or not.  Done right, I think it is a very good thing.  Done poorly, this could be a disaster.

Here's the deal. In Angular 1, when you wrote a directive, any CSS you wrote to go with it needed to be included separately by the person using your component.  In Angular 2, you bind that CSS to your component and it gets included in the HEAD of your html.  It also gets mangled, similar to how ASP.NET would mangle IDs in WebForms, so that the CSS from the component won't conflict with the other CSS that the page is using.

This is all in preparation for the forthcoming HTML standard that will support Web Components. What is good about this is that the component is stand alone.  What is bad about this is that if I need to override the CSS, that might be a bit more difficult to do than what we are used to.

### Routing

My biggest disappointment with Angular 2 is with the new router.  Is it better than the router that comes with Angular 1?  Yes.  Absolutely.

Is it better than [UI-Router](//angular-ui.github.io/ui-router/)?  I’m not so sure.

The new router will let you have nested routes.  While it doesn’t do it in exactly the same way as you can with UI-Router, it can be done.  And like UI-Router’s ng-sref, you can link to routes by name.

But the part I never could find clear documentation on is how I would create two insertion points in my HTML for route specific content like I could do with UI-Router.  From what little I could find it looks like they had something working that looked a lot like UI-Router and then they ripped it out in favor of some other method that either is not there yet, or is not clearly documented.

### Singletons

If you’ve done any work with Angular 1, you’ll be familiar with the confusion between the different types of singletons.  The good news with Angular 2 is that everything is just considered a service.  If you decorate your class with “Injectable” the class can be injected into other classes. But, and I consider this a really big stumbling block moving to Angular 2, services are really singletons.  This is both good and bad.

Typically we create a hierarch of components in our site with components housing other components.  To get singleton behavior, you would declare the need for your service as far up that hierarchy as is practical.  Just not at the app level.  You would hardly ever want to do that.  You only declare the dependency once in your app.  At the point you declare the dependency a new object is created that is injected anywhere else down the component hierarchy you specify that you need it in your constructor.

But you need to pay attention to how I phrased that last paragraph because what creates the object is the fact that you declared the dependency.  If you declare it again, you get a new object.

At first glance, this may appear to be a bad thing.  But I can think of situations where I may actually want to either have one object for an entire application or one object per instance of a component.  This new way of implementing injectable objects gives us that flexibility.

### Promises vs Observables

While it doesn't make sense to get into the details here, it is worth mentioning that instead of using Promises to avoid callback hell, Angular 2 is using RXjs' observables.  It might take a while to get your head around this new paradigm.  But as it turns out, it would appear that observables can be used in more places than promises can because they are stream based.

Better/New/Enhanced
-------------------

### Declarative or Explicitly Coded Forms

In Angular 1, just about everything view related is declarative.  This works, but at times I found it awkward to use.  For example, I needed to add validation to an element and it just seemed there must be an easier more direct way to do this than what I was given in Angular 1.

In Angular 2, you can just put enough code in the HTML to render the element and then attach the JavaScript to it using function calls.  This gives you A LOT more control over you view, even if it does take a bit more work to get it setup.

I think there are times when it will still make sense to do everything declaratively.  But for anything more than a trivial form, I think you are going to love this new option.

### Binding Optimization

The Angular 2 databinding seems to be optimized.  I don’t fully understand exactly what is different, but I’ll take their word for it that it is, or will be once we are no longer in beta.

Still Needs Work
----------------

### Rendering Optimization

As I’ve started to look at React, and other systems that create “Virtual DOMs” I keep thinking, “Why can’t Angular do something like this?”  Being able to manipulate all of the DOM and then show the result when you are done is one of the easiest places to improve performance, and yet I don’t see any hooks in Angular that would make this possible.  This isn’t to say that I think React is better than Angular.  So far, I see some really big issues with React as well.  But I’d love to have some kind of Virtual DOM implementation in Angular without losing what is already built in.

### Testability

I can’t comment fully on the ability to test Angular 2 simply because the documentation for this is not fully baked.  So, I’ll refrain from making any comments at all.

Conclusion
----------

There are a lot of wins with Angular 2 and I think, in general, you are going to like this new framework.  It will be interesting to see how the UI component space will mature around this new framework.

If you are interested in learning Angular 2, I would suggest that you work through the [tutorial](//angular.io/docs/ts/latest/tutorial/) and read through the [development guide](//angular.io/docs/ts/latest/guide/).
