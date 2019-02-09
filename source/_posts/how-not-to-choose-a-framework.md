---
title: How Not to Choose a Framework
tags:
  - angular.js
  - ext js
  - javascript
  - react.js
url: 3922.html
id: 3922
categories:
  - Programming Best Practice
date: 2016-06-02 06:30:00
---

In my job as a JavaScript architect, trainer and mentor, I’m often asked, “What’s your favorite framework?”  Or “What is the best framework?” And it surprises people when I give them two answers to that question. Right now, of the frameworks I’ve looked at, my favorite framework is [React JS](/tags/react-js/).  But if I were picking a corporate framework, at this point I’d probably land on [Angular 2.0](/angular-2-first-impressions-compared-to-angular-1/). But the question you are probably asking is , “Why two different selections?”  And, I think a more interesting question would be, “How did you select which one to use?” In fact, when I was thinking about writing this post, I was going to title it “How to Choose a JavaScript Framework” but as I considered what I would actually say, I realized that the factors I would use really apply to any language and any time. But an even more interesting question is this.  What factors are essential when picking out a framework.  If I ignored these questions, what are the cost? So, I give you… <figure>![](/uploads/2016/05/image-3.png "How Not to Choose a Framework")<figcaption>Photo credit: [Tony Webster](//www.flickr.com/photos/diversey/980101167/) via [Visual hunt](//visualhunt.com) / [CC BY](//creativecommons.org/licenses/by/2.0/)</figcaption></figure>

<!-- more -->

How Not to Choose a Framework
-----------------------------

As we progress, I’ll mention frameworks I have experience with.  To be fair, I will let you know my preferences. In order, the frameworks I would prefer to use would be:

*   React JS
*   Angular 2
*   Angular 1
*   Ext JS 5 or 6
*   Ext JS 4

If I were going to recommend a framework for a large enterprise organization, the order would be slightly different:

*   Angular 2
*   Angular 1
*   Ext JS 5 or 6
*   React
*   Ext JS 4

With this in mind, here’s what you should pay attention to.

Who Will Use The Framework?
---------------------------

At the organization I am working at now, most of the people there are Java programmers.  This means that programming in JavaScript, of any flavor, is going to be as much as a mind-shift as moving from C to C++ or C++ to Java or C#.  Yes, there are similarities to what they are used to, but there are enough differences to consider that you’ll probably gravitate toward a framework that allows them to not have to worry about those differences.  On this point Ext or Angular 2 are probably going to rise to the surface because they, more than any other framework, allow you to work with JavaScript more like it was like Java or C# than any other framework I know about.

How Steep is The Learning Curve?
--------------------------------

Related to who will use the framework is how long will it take them to learn the framework?  For this, you are going to want to look at things like:

*   Can I buy support so I can get my questions answered?
*   How clear is the documentation?
*   How popular is the framework?
*   Do I already have an expert on my team?
*   Is there a public Slack channel for this framework?
*   Do the people behind the framework care about the Enterprise?

On this point, depending on the experience of your developers, Ext JS and Angular 2 are probably going to surface as the clear winners while React is going to end up at the very bottom.  As much as I love it personally, I have to admit that learning it has taken me the most amount of time.

How Opinionated is the Framework?
---------------------------------

I remember when VB 1.0 was first introduced.  The reason everyone gave me for why I should love this new development environment was, “Look how fast I can get something up and running.” Well, yes, but… VB let you write code any way that got the job done.  And coming from C++ and MFC and prior to that, OWL from Borland, I recognized that even though you could still write crappy code using a framework that provided some structure, the amount of crappy code you wrote was inversely proportional to the amount of structure the framework provides. When you are working with a large team of developers, something needs to be in place to ensure they are writing code in a highly structured way rather than just getting the job done. Once again, this places Angular 2 at the top of the pile of the ones I’ve actually worked with.  Ext sinks to the bottom of the pile.  While Ext does implement something they call MVC and MVVM, they don’t protect the developer from coding outside of what those design patterns are supposed to look like.  In the case of MVC, I’m not even sure the people who wrote it know what MVC is.

Industry Standards
------------------

The easiest way for me to illustrate what I mean here is to point out a few ways this gets violated with the existing frameworks. In order to make Ext work more like a desktop development environment, they generate the HTML for you and use their own layout mechanism to control where the various elements appear on the screen.  Every other framework I’ve mentioned lets you control the layout using CSS.  The advantage to Ext is that I don’t have to know HTML or CSS in order to get a screen up that looks nice.  The down side is that if I want to do anything just a bit out of the Ext box, I quickly become frustrated.  It also takes more time to render a screen than if I were using HTML and CSS.  This is particularly true if your components are nested more than 3 deep. Further, Ext has enabled JavaScript to look more like Java and C# than JavaScript by implementing a proprietary mechanism for defining a class.  What continually worries me is how well this will continue to work as the ECMAScript standards evolve and provide there own mechanisms for achieving the same results. Ext also (sorry, but Ext is the primary violator of this point) uses its own proprietary build process.  It is possible to circumvent their build process for most things.  But the question one has to ask is, “why can’t you just use standards like gulp, grunt or npm scripts?” Even though Angular 2 primarily uses TypeScript, the difference between Angular 2 and Ext is that 1) you don’t HAVE to use TypeScript even though it is highly encourages and 2) TypeScript only implements features that look like they are going to eventually end up in the ECMAScript standards.  So, it is a lot more future proof while adding a lot of the same features that Ext implements in a more proprietary fashion. Another framework where this kind of shows up is with React JS.  While everything about building the app is built using industry standards, the unit testing framework doesn’t allow you to use Karma as your test running.  There is another more proprietary implentation called Jest.  However, I also don’t have to use PhantomJS to gets my components.  I wish I could have both.

How Testable Is It?
-------------------

Anyone who is familiar with my history of posts, who know me personally, know [I am a huge proponent of TDD](/tags/tdd). So, any framework I use has to allow me to unit test. This is why Ext JS 4 ends up dead last on my list. You would think that a framework that says they implement MVC would allow you to test the controller without have the view attached. That’s one of the points of MVC. But Ext doesn’t allow this. On the other end of the spectrum, React is testable all the way down. This is why I love it. The only reason I don’t consider it the right choice for the enterprise is because it takes so long to learn and the documentation isn’t very well done.

Doing The Research
------------------

OK.  So, this is what you should look for, but when you are looking at the frameworks, how would you know?  Most of this information is only stuff you would find out after your programmers started using the framework. One way you can find out is to find people who have used the various frameworks you have under consideration.  One of my first test would be, “How much information can I find on the Internet about this framework?”My second question would be, “How popular is this framework?” and then finally I would look for people who don’t like the framework and try to determine if their points are valid.

How Not to Pick a Framework
---------------------------

If you want to pick the wrong framework, trust the sales literature.  Don’t ask any question.  Ignore the points above. Ultimately regret your decision.
