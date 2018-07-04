---
title: Secrets to Styling Angular2
tags:
  - angular 2
  - animation
  - css
url: 4192.html
id: 4192
categories:
  - Angular 2
date: 2017-01-31 07:30:00
---

This past week, while working on a new project, I discovered some secrets to styling Angular2 that I don’t think are very well known. There are two specific issues I needed to solve this week that took a bit of digging.  The first was that I wanted my routes to fade in and out as I move between routes.  The second was that I was using a grid control from a third party and I needed to style an inner component.  We will cover both as well as some more basic operations. <figure>![](/uploads/2017/01/image-2.png "Secrets to Styling Angular2") Photo via [Visual hunt](//visualhunt.com/)</figure>

<!-- more --> 

Angular2 Version
----------------

Just so there isn’t any confusion, this article is accurate for Angular 2.x as of 2.5

The Basics
----------

Just to make sure we have the basics covered, we want to start with generic styling.  The temptation might be to style each of your components independently.  This would be a mistake.  Angular2 doesn’t throw out all the existing CSS rules.  Instead, it adds to them.  Therefore, anything you can do with a generic style should be handled at that level. Since I generally use Bootstrap to theme my applications, this is what I’ll reference here.  Using the angular-cli, the way you would add the CSS theme information is to include it in the angular-cli.json file of your application in the “styles” section.

...
      "styles": \[
        "../node_modules/bootstrap/dist/css/bootstrap.css",
        "styles.css"
      \],
...

If you have a component that you need to style in a way that is outside the bounds of the general CSS you’ve included, you can add CSS to the components CSS file.  This all works as expected with one small exception.  The CSS you add to this file only applies to the component and will override any other CSS that might already be applied by the general CSS. This is Angular2 CSS 101 stuff.  I’ve written pretty extensively about this in the article “[Adding CSS and JavaScript to an Angular-CLI Project](/adding-css-and-javascript-to-an-angular-2-cli-project/)”

Host access
-----------

But there are times when the thing you want to style is the host container of the component.  Not just the HTML inside it.  There are two ways that you might do this.

### :host

The first way is to use the `:host` directive in your CSS.  But you’ll need to be careful with this, as I found out recently. You see, you might expect that

:host {
    background-color: blue;
}

would cause the background color of the element to turn blue.  But if you try that with a simple component you’ll find out that nothing shows up with a background color of blue because by default an element that isn’t part of the HTML spec has no size and basically disappears from the display.  We want it to display an essentially be a container for all our other HTML so what you really want to do is something like:

:host {
  background-color: blue;
  height: auto;
  width: auto;
  position: absolute;
}

The only way it will show up, in my experience, is to make the position ‘absolute.’  Remember, we are doing this only because we want the :host to have some impact as a container for all of the other elements that might be in it.  Normally, you can get by styling the html inside it.

### @HostBinding()

An alternate way of setting style on the component container is by using the @HostBinding() decorator.  What this does is that it binds a variable to the containers attribute so that you can change the value from your TypeScript code.

@Component({
  selector: 'app-view',
  templateUrl: './view.component.html',
  styleUrls: \['./view.component.css'\]
})
export class ViewComponent implements OnInit {
  @HostBinding('style.backgroundColor') 
    backgroundColr = 'blue';
  
  constructor() { }

  ngOnInit() {
  }

}

Child Elements
--------------

Now, the other problem you might run into is that you’ll be using some third-party control and you’ll want to style some container element inside of it to fit your needs.  Again, this isn’t a particularly common problem, but it might just take you a while to find the answer. The first thing you may try is to just style the markup.

component-parent component-child {
    /\* style stuff here */
}

But, try as you might, you’ll never get those styles to show up.  You can style the `component-parent` all you want, but not the `component-child` no matter what you do. Here is the trick that allows you to style the `component-child`:

component-parent >>> component-child {
    /\* style stuff here */
}

That `>>>` thing is call the “piercing” operator.  All you need to know is that it is how you get the child elements styled.

Animations
----------

You might wonder, at first, why we would need an Animation API in Angular2.  Aren’t CSS animations good enough? Well, actually… It isn’t that CSS animations aren’t good enough, but because Angular2 “hide” and “shows” elements by putting them into and out of the DOM and does other DOM manipulations that conflict with CSS animations, you will find there are time you are going to need to use the Angular2 Animation API. The Animation API works in a similar way to how the CSS Animations work so this isn’t going to be a big stretch for you if you are already familiar with CSS Animations. To animate a component, you are going to need to add an animations section to the @Component decorator of your component:

@Component({
  selector: 'app-view',
  templateUrl: './view.component.html',
  styleUrls: \['./view.component.css'\],
  animations: \[
      /\* animation definitions here */
  \]
})

The question then, is “how do we define an animation” Since animations is obviously an array, we want to know, what is it an array of?  It is an array of triggers.  A trigger has two parts.  A name, an an array that specifies how the animation should work.  This name gets used in our template using the \[@name\] syntax to bind to a component property. Next, we define each of the states we want to respond to and style we want to end up with when that state is triggered. Finally, we define each of the transitions. The combination gives us a lot of flexibility.  While we can create the same kind of transitions that we might have using CSS, we can also create transitions the we never could have using CSS.  All we need to do is have our component change the value of some member variable in some predictable way and the transitions will kick into work. You can read more about transitions on the [Angular2 documentation page](//angular.io/docs/ts/latest/guide/animations.html).

Route Animations
----------------

Route animations are similar.  However, this is where everything I’ve said above comes together. The first problem you are going to run into trying to animate a route is that the state you want to base your animations on is the route, which is the parent component of your component you are probably trying to animate.  But the reality is, what you are really doing is animating something when it is first displayed and animating it again when it is going away.  That is, when it is placed into the DOM and when it is being removed from the DOM.  Angular2 has to predefined states for this, ‘:enter’ and ‘:leave’. So, we create a trigger named ‘routeAnimation’ and in our route components we bind to it using the @HostBinding() decorator.

@HostBinding('@routeAnimation') routeAnimation = true;

Everything else you need to know I’ve already discussed above. For more information about routing animations, you can check the [router documentation on the Angular2 documentation page](//angular.io/docs/ts/latest/guide/router.html).

Finally
-------

It took me way too long to figure this out because a lot of the information has changed since RC0.  Hopefully, it will point you in the right direction.
