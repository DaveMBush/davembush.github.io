---
title: 'Amazing Angular2 DOM Tips, Tricks, and Warnings'
tags:
  - angular
  - DOM
  - typescript
url: 4180.html
id: 4180
categories:
  - Angular
date: 2017-01-17 07:30:00
---

I’ve been working with Angular2 now since RC0 and I’ve learned quite a few things about Angular2 DOM tips, tricks, and warnings that you’ll want to pay attention to as you get started. <figure>![](/uploads/2017/01/image.png "Amazing Angular2 DOM Tips, Tricks and Warnings")<figcaption>Photo credit: [Sister72](//www.flickr.com/photos/sis/196867770/) via [VisualHunt](//visualhunt.com) / [CC BY](//creativecommons.org/licenses/by/2.0/)</figcaption></figure>

<!-- more --> 

Avoid DOM Manipulation
----------------------

One of the first things you need to understand about how Angular2 works compared to AngularJS is that any DOM manipulation you do using Angular2 isn’t really manipulating the DOM.  At least not directly.  Everything that happens at the DOM level is a result of a state change in the component.  When Angular2 realizes that the state has changed, it changes the DOM to reflect the change. What this means, generally, is that if you need to know about some state change that happened at the DOM level, you’ll want to track that change in your component class.  Can you access the DOM directly from your code?  Yes.  And sometimes you might just have to.  But you’ll produce code that is much easier to test if you avoid having your code reach up into the DOM to get current state information. You may have already heard about the experimental Web Workers support in Angular2.  To make sure you can use this, it would be best to avoid direct DOM manipulation until we are sure the Web Workers implementation will be able to deal with this properly. What this all means in practical terms is that you’ll want to avoid using libraries such as jQuery to manipulate your code and instead replicate that behavior using native Angular2 calls.

Only Generate the HTML You Need
-------------------------------

On a similar note, you only want to render the HTML you’re going to need at any one time.  Not everything all at once.  This will allow your code to render much more efficiently. For example, if you have a menu that has dropdown menus, the temptation is going to be to render all the HTML the menu may need all at once and use JavaScript to show or hide the dropdowns as you mouse over them or click them.  But with Angular2, you could use the \*ngIf directive to include and exclude the menu option as you need them to show.  This reduces the initial page size and simplifies your code.  Been there, done that. By the way, there are other directives you’ll want to get familiar with, but \*ngIf is probably the most often ignored because you are likely to try to use display:none to hide an element instead of just eliminating it from the DOM completely.

Minimize Change Detection
-------------------------

I was recently working on a component that displayed a nested array as a set of tabs and dropdown menus.  Everything was working great but I wanted to check the CSS on the dropdown so I could make some adjustments.  This is when I discovered that the HTML was being rewritten about once a second even though nothing had changed.  I couldn’t see this on the main screen, but it became super evident when I opened the developer tools. Fortunately, I had already learned about OnPush change detection.  So I was able to set my component to use Push notification:

@Component({
    templateUrl: 'template.html',
    changeDetection: **ChangeDetectionStrategy.OnPush**
})
export class View ...

And now the component only re-renders when the data it is looking at changes. For Push notification to work correctly, all the data that the component is looking at has to be [immutable](/what-if-everything-was-immutable/) or an [Observable](/reasons-to-use-rxjs-today/).  These are both patterns you should become familiar with because any well architected Angular2 application will make significant use of both of these.

Accessing the Component Container
---------------------------------

Another problem I recently had was that I wanted to use the class attributes from the container on a child component.  The question I had trouble getting the answer to was, “just how to I access the container element?”  This was very easy.  It is always easy once you know the secret handshake.

this.elementRef

There are various properties and methods hanging off that which might be useful to you.  In my case, I wanted to go after the classes that had been attached and reattach them to the INPUT element that was a child of the component.  So, I needed to use:

this.elementRef.nativeElement.classList

Accessing Child Elements
------------------------

Similarly, you might want to access child elements from your code.  This is much easier to find when you search the Internet.

@ViewChild('input') input: ElementRef;

@ViewChild is an attribute that tells the Angular2 compiler to look for an element in the template with the variable named input.  I’ll explain template variables in a bit.  In the code above, I’m going after the INPUT element with a template variable named “input” so I typed it as ElementRef since I don’t have a specific class name for it. If you only have one unique element, you can just use code that looks more like this:

@ViewChild(ElementClassName) variableName: ElementClassName;

Of course, you’ll want to make sure you imported ElementClassName for this to work.  In this case, I’m going after a specific type of element so I type the variable as that type.  Now my typescript code knows what properties, fields, and methods I have available.

Template Variables
------------------

As I mentioned above, you can create template variables to allow you to access your child elements from your typescript code.  But you can use them for other purposes as well.  To create a template variable, just put a hash in front of the variable name you want to use.

<input #firstName ...>

Now you can use this in your typescript using ViewChild() as explained above, or you can use it in your template as a regular variable.

<div>{ {firstName.value}}</div>

Use a Model Driven Approach
---------------------------

If you are coming from AngularJS, you may be tempted to use what is commonly referred to as a Template Driven approach.  This is approach that relies no “Two-Way” data-binding to update the data in the typescript code to the fields in your template.  While this works for a lot of simple apps, you never know when a simple app will turn into a complex app and your Template based approach will quickly become insufficient. No, what you want to use is the Model Driven approach.  This approach gives you A LOT more control over what we typically are attempting to do in our forms and scales well as the complexity of our forms increases. I’ve written a lot more about this in my post on [Angular2 Architecture](/awesome-angular2-architecture-options-and-opinions/).

Conclusion
----------

Obviously, there is a lot to learn.  If you keep using Angular2 like you’ve been using AngularJS, you are going to run into a lot of difficulties.  It pays to learn how a system works prior to using it for a real application. What have you learned along the way?  Leave a comment.
