---
title: More Control with Angular Flex Layout
tags:
  - angular
  - css
  - flex
url: 4284.html
id: 4284
categories:
  - Angular
date: 2017-04-11 06:30:00
---

If you are using Angular(2+) and you are looking for an easy way to layout your components that gives you lots of flexibility and very few restrictions, you owe it to yourself to checkout [Angular Flex Layout](//github.com/angular/flex-layout).  While it is still in Beta, the framework is quite usable.  I’ve been using it in one of my projects and I’ve been quite happy with the results.

<figure>![](/uploads/2017/04/image-2.png "More Control with Angular Flex Layout") Photo via [VisualHunt.com](//visualhunt.com/re/7d8037)</figure>

<!-- more -->

## The Old Days

### Straight CSS

I tell people, “I’ve been programming websites since ‘Al Gore invented the Internet’” Which is to say, some time prior to 1998. So, I’ve had to deal with layout issues for a very long time. At least in terms of “Internet Time.” And during that time, being able to lay things out on the page in some sort of intelligent way has matured quite a bit. But still, coding it all by hand, unless you are a fulltime CSS person, is not the best use of my time.

This is, in part, why Bootstrap was created.

### Bootstrap

I generally love Bootstrap. A great CSS framework that allows me to style my application easily, is easy to modify, and has a pretty respectable grid systems for placing my controls on the page. The problem with Bootstrap though is that the grid system is really all you have available for layout, and it is rather limited. At least that has been my experience. By default, you get a grid with 12 columns. And while you can next grids in other grids, you still end up with alignment issues. There must be an easier way.

### AngularJS Material Design

The Angular Flex Layout used to be part of the AngularJS Material Design project. But instead of making it part of the Angular Material Design project, it has been broken out so that we can use it in combination with other systems. Grids where they make sense and Flex Layout where that will work better.

## Flex Layout Benefits

### No Grids

Now, what makes Flex Layout so great? The first thing that I think of is that I can do everything I was able to do with the Bootstrap grid system, but I have a lot more control. In fact, this past week, I converted an existing layout that was using nested grids to achieve the layout I was looking for and flattened it significantly by switching it to use Flex. You see, in Flex, you can say you want a row and then for each cell in that row, you can specify the width of the cell in any unit you want, or you can tell it to take up the remaining space. And just like Bootstrap grids, the DIVs wrap if needed.

### Directive Based

The other thing that is true of flex is that all of this control is specified at the template level. I’m not specifying layout in a file that is separate from the template I want to apply it to.

Now, the CSS purist might object to this. “Style information should all go in a global CSS file so you aren’t repeating yourself.” They’ll say. 

Well, yes, that’s true, if you are creating multiple pages that you want to all look the same. But we are talking about Angular here. A Single Page Application. If you have multiple templates that all need the same layout information, you are probably not thinking about your components correctly. That is, your problem isn’t a CSS/Style issue, it is a component issue.

In a SPA, each page looks the same because each page uses the same parent component. Rarely, do you really have a need to share layout styles. Mostly we share component look and feel. That’s different.

### Responsive

And if I want a cell to be one size for desktop and another size for tablets or phones, I can easily specify what size each should be. Similar to how we would do it with Bootstrap, but in much finer detail.

## It’s Just CSS

Before we move on, I want to point out that Flex Layout doesn’t do anything that the current CSS spec doesn’t already allow us to do. However, the current Flexbox CSS implementations are so new that each browser implements the spec enough different that we can’t be sure the styles will work the same way as we move from browser to browser.

What Flex Layout attempts to do is to normalize the differences in a way similar to how jQuery normalized the DOM for us. Someday, we may not need Flex Layout. But until then, this is going to save you a lot of time trying to figure things out.

## What You Can Do

### Maintain Aspect Ratios

I first started playing with the Flexbox CSS spec when I needed to implement a layout that included a video player that was bounded by a splitter control. As the splitter resizes the panel, the video needs to shrink and grow maintaining the aspect ratio while at the same time allowing the cell under it to grow and shrink. This is something that Flex can handle easily.

### Rows with Cells

As I’ve already mentioned, it is as easy to set up a new row and place wrapping DIVs in it as it is with Bootstrap.

### Columns with Cells

But unlike Bootstrap, you can also create groups of Columns with Cells.

### Responsive Card Layouts

The last couple of weeks, I needed to implement a card layout that changed the number of columns displayed based on the width of the container. By using Flex Layout along with min-width and max-width on the cards, I was able to get this to layout correctly regardless of the container width.

## Getting Started

I’m not going to spend a lot of time going over the ins and outs of using Flex Layout. They do a pretty respectable job on the website. But, the one thing I did have trouble getting started with was exactly what syntax to use. Some stuff I found used `fx-flex` syntax and other sites used `fxFlex`. The one you want is `fxFlex`. That and a bit of experimentation should get you well on your way.
