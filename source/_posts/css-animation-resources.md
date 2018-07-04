---
title: CSS Animation Resources
tags:
  - animation
  - css
  - html
url: 2692.html
id: 2692
categories:
  - CSS
date: 2014-10-30 06:00:00
---

loading...

This week I discovered CSS Animations.  Well, I shouldn’t say I discovered it so much as I finally spent some time figuring out what it is and why I would care about it.  This could make so much of what we normally do in JavaScript entirely unnecessary.  So, more for my own benefit than anything else, I thought today I would just create a list of resources that are available. But first, why would you want to use CSS for animation when JavaScript could do it just as well?

CSS and HTML live in the same world
-----------------------------------

As I mentioned a few weeks ago when I discussed JavaScript performance, anytime you manipulate the DOM from JavaScript, you have to cross the HTML/JavaScript boundary.  And to do it right, you have to make sure you work in sync with the key frames that the DOM is living in.  If you don’t, at best you will be animating more than you need to if you use JavaScript to do the animation.  On a phone, this can eat up precious battery life.  For me, this is the biggest reason.

Declarative Code Is Easier To Read
----------------------------------

I realize that some may disagree with this.  But my feeling is, if I can express in one line what would  take a loop in JavaScript to do, I’ll take the one line of code.  This one of the things that makes Angular attractive.  I can create my own HTML tags.  Why not get the same benefit from my CSS?

Reasons Why You Might Not
-------------------------

There are a few reasons why you might not use CSS Animations.  First, it is relatively new.  It may not do everything you want it to do.  So, you may have to resort to using JavaScript for at least some of the things you want to do. Second, it may not support all of the browsers you need to support.

Resources
---------

Here are a brief list of resources that I found

### CSS Generators

*   [CSS Animate](//www.cssanimate.com/)
*   [CSS3 Maker](//www.css3maker.com/css3-animation.html#)

### CSS Animated Accordion

*   [Bradshaw Enterprises](//css3.bradshawenterprises.com/accordions/)
*   [James Tease](//www.jamestease.co.uk/blether/accordion-using-css-animations)
*   [Martin Ivanov](//martinivanov.net/2014/03/19/animated-css3-only-horizontal-accordion/) (side ways)
*   [The CSS Ninja](//www.thecssninja.com/css/accordian-effect-using-css)

### Menus

*   [CSS Menu](//css3menu.com/) (lots of variations)
*   [Menu Maker](//css3-menu.com/animated-css3-menu.html)

### Slide Show

*   [Infinite Scrolling](//css-tricks.com/infinite-all-css-scrolling-slideshow/)
*   [One at a time](//agskryp.wordpress.com/2014/07/20/css3-animation-slideshow/) ([http:/oneatatimeslideshow](/oneatatimeslideshow "http:/oneatatimeslideshow"))

When I first suggested “CSS Animations” to a designer friend of mine, he reacted as though I was suggesting putting a flash animation on a web site.  Obviously, we wouldn’t want to do that. But we use animation is some pretty creative ways.  So the next time you reach for JavaScript to add some sort of animation to your web application, do a search for a CSS Animation instead.
