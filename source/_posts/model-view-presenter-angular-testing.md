---
title: 'Model View Presenter, Angular, and Testing'
tags:
  - angular
  - mvp
  - unit test
url: 4426.html
id: 4426
categories:
  - Angular
date: 2017-08-29 06:30:33
---

While testing Components is possible, it is not easy and is often pointless.  Using the Model View Presenter pattern, or a variation of it, solves the problem. Here's the deal. Long time readers of my blog know I've been a proponent of Unit Testing for a very long time. While I was learning React, I went through the exercise of trying to write test as I was learning.  Now, the great thing about Angular and React is that it is possible to test your components.  The problem with testing components is that you are either testing that your HTML ended up in the right spot, that Angular directives did what they should, or you are evaluating the DOM to verify that component logic worked.  In most cases, putting tests that do any of these at the component level is the wrong way to test. <figure>![](/uploads/2017/08/2017-08-27.jpg "Model View Presenter, Angular, and Testing")<figcaption>Photo credit: [Tamworth Borough Council](//www.flickr.com/photos/tamworthboroughcouncil/15657023428/) via [Visualhunt.com](//visualhunt.com/re/e21893) / [ CC BY](//creativecommons.org/licenses/by/2.0/)</figcaption></figure>

<!-- more --> 

Evaluating HTML
---------------

Ignoring for a second that setting up a component test is awkward, the question I want to address here is, "is that really an appropriate test?" If all your smart component does is pass data down to dumb component, all you really need to verify is that given a dumb component with an html fragment, and another one with another fragment, they will all end up sequentially after each other.  It is a pretty easy test to setup. But all you've ended up testing is that Angular does what it says it will. It won't tell you what you can't already see by running the code. Dumb components are even more obvious.  Let's go for something obvious.  You have a ngFor that allows you to display a list of HTML.  You setup your component so that it has an array of three known items, you pass that into the component, do a change detection cycle, and verify that your HTML displays as expected. Great, you've verified that Angular works again.  You will have a hard time convincing me that you've really tested anything.

Angular Directives
------------------

In this case, you are going to try to verify that when you click on a component, or pass it some data, or... whatever, that Angular does what it should.  Maybe you do need to verify that when you click an element, something else happens.  But this is not the place. You might create an integration test, which would, by definition, take longer to run.  It would be better if you tested this using an end to end test.  But testing this as a unit test, doesn't tell you much more than that Angular has said that it would. "But, I need to verify that the code in my event handler does what it should!"  You might complain.  Yes, you do, but you don't need to fire a click event to do that, just call the event handler.

Big Fat Hair Logic
------------------

And then there is that "big fat hairy logic" issue.  You've created some sort of component that has some rather complex logic.  OK, that happens.  A grid control is a perfect example.  But, maybe your logic is in the wrong place?

A Better Solution
-----------------

There is a design pattern called "Model View Presenter" Like all of the MV* patterns it aims to separate out logic from the view so that we can test things easier.  It was popular with WebForms in ASP.NET when that was how you wrote ASP.NET web sites.  The way this worked was that you would create an Interface for your WebForm that represented all of the things you wanted to have access to from your logic code.  Your presenter.  All your component or page code did was respond to events on the page and expose data to the data driven forms.  In a lot of ways, Angular isn't much different from WebForms.  Your TypeScript file is essentially a "code-behind" file and your template is similar to an ASPX page. The beauty of the MVP pattern is that when you do it right, your component has no logic at all.  It renders data and responds to events by calling down to the presenter.  In an Angular world, I doubt the Presenter would ever need to call up to the view.  This is prefect.  Now I can create a test for my component logic in a way very similar to how I would test any other Injectable.  Because Injectables are what we are going to use here.

### Injectable Presenters

For the point of illustration, let's assume that all components would follow this pattern.  Now a simple component would have four files instead of the normal three.

*   *.component.ts
*   *.component.html
*   *.component.css
*   *.component.presenter.ts

The *.component.presenter.ts file is our new Injectable.  Any properties in our component would pass on down to the presenter.  Any methods, which should be few to none, would pass on down to the presenter.  The presenter is where we do all the work.

### Make the Presenter Available to the Component

Now, if you've studied the Angular tutorials, you probably already know this, but my bet is most people programming the new Angular don't.  You can make an Injectable available by providing it in a module, or by providing it in a component.  If you provide it in the module, it is globally available.  If you provide it in the component, it is only available to that component or its child components.  This is perfect for our use case here.  So, we provide it in our component and then inject it into our component's constructor. Everything else we might have injected into our component can now be injected into our presenter.

Look Ma, No DOM
---------------

The side effect of this pattern is now, the complexity of the methods in our component should be 1.  This means they don't really need to be tested.  And because they don't need to be tested, we don't really need a DOM available to run our tests.  This makes it MUCH easier to use jsDOM to run our unit test without having to wire in a bunch of polyfills just to make it all "work."

What Do You Think?
------------------

I'll admit, I've been chewing on this one for several weeks.  I've run it past some other programmers I respect and I've yet to find any holes in this architecture. Some questions I'm still thinking about:

*   Should we write every component like this for consistency or should we only do this once a component has a method that actually has some logic?
*   Is there ever a case when a Presenter would need to access the Component or can we safely assume that the pass-through mechanism would be enough?
*   Did I miss something?

Let me know in the comments below.
