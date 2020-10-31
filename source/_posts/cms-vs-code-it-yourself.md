---
title: CMS vs Code It Yourself
tags:
  - asp.net
  - cms
  - dotnetnuke
  - WordPress
url: 1051.html
id: 1051
categories:
  - Opinion
date: 2013-08-14 08:42:39
---

![trav-064](/uploads/2009/04/trav064.jpg "trav-064") This post has been percolating in my brain for several weeks now and I think it’s finally at the point where it’s “done.”  Well, see…

The problem area is this.  At what point and under what conditions would you write the code yourself vs. using a content management system?  And if you were to use a content management system, which one should you use and why?

Along the way I’ll tell you what my current choices are, but more importantly, I’ll tell you what my thought process is.  So even if you decide to use different tools than I do, you can ask the same questions to select the tools you have decided to use.

<!-- more -->

## My Primary Tools

I think in order to give the rest of this post some context, it makes the most sense to tell you what my current tools are.

1.  WordPress
    1.  Linux/Apache
    2.  Windows/Apache
    3.  Windows/IIS
2.  DotNetNuke
3.  Write it myself.

## Advantages of WordPress

I’ve actually had people ask me why I use WordPress for my .NET blog instead of some .NET blog engine.  The primary reason is, it has been around longer so it has more plug-ins and more themes available than anything I can find for .NET.

What this means to me is that I can achieve a lot of goals by simply searching for an appropriate plug-in.

Just by way of example, here are some plug-ins I’ve used.

*   Plug-in to allow me to change the Title tag for each page from the “standard” title.
*   Plug-in to create a forum on my site
*   Plug-in to allow pages to redirect to other pages.
*   Plug-in to automatically cross-link my post
*   Plug-in to automatically cross-link my site with other sites (not using that here… yet)
*   Plug-in to run a slide show
*   Plug-in to keep out spam
*   Plug-in for forms creation.
*   Plug-in for podcasting.
*   Plug-in for increased role-based access to just about every imaginable module in WordPress.

So you have the ability to easily create anything from a blog to a brochure site to e-commerce (yes, there are e-commerce-related plugins) without having to write any code.

While there are several themes that you can pay for, there are so many themes that are free that I’ve never had to buy one of the “premium” themes for any of my sites.  It does help that I can tweak the themes.  But they generally work out of the box.

Finally, I find that my clients find WordPress sites easier to manage than DotNetNuke.

## Disadvantages of WordPress

You are limited in how flexible your UI is compared to my other two options.  But I rarely find this to be a problem.

I don’t consider myself a PHP programmer, so I can’t create any new plug-ins I might need.  I can hack the PHP, and anyone who’s familiar with ASP.NET probably could at least hack PHP as well, so don’t let the fact that you don’t know PHP keep you from a PHP solution.

It also doesn’t run as well under Windows as it does under Linux.  If all you know is Windows, this might be an issue for you.

## Advantages of DotNetNuke

There are two main advantages of DotNetNuke over WordPress that would make me choose it over WordPress in most situations.  First, I know .NET, so if I need to write my own module, I can do that.  Second, it allows much greater freedom in HOW things are presented on the page.

There are also quite a few skins available for DotNetNuke, however most of them you have to pay for and I’ve never put up a site yet that I didn’t buy a theme for.

## Disadvantages of DotNetNuke

There are two huge disadvantages to DotNetNuke as well.  The first is that it is harder for my clients to manage.  In fact, while managing a DotNetNuke site is easier than having to change code, I would suggest that you not even tell the end user that they can manage the site.  You might expose specific pages to them, but leave the overall administration to people who understand DNN.

Second, the level of granularity of role based permissions is less than the plug-in for WordPress gives me, and I’ve yet to find a module for DotNetNuke that gives me the same level of granularity.

Finally, because you can mix and match skins and containers in such a way that it is difficult for the skin designers to test every possible mix, I find that every skin I’ve used has needed to be tweaked.  Unlike the WordPress themes that just work.

## Write it Yourself

Of course, the advantage to writing all the code yourself is that you have complete control.  The disadvantage is that you have to write everything yourself, including the skin.

## A Word about Skins and Themes

As recent as 3 years ago, I used to have the presentation layer designed for me by a designer.  This process typically cost between $5,000 and $10,000 depending on how many iterations we went through.

However, I’ve come to the realization that design matters very little.  Yes, the site has to look good.  Yes, it has to be usable.  But it doesn’t have to be unique from every other design.  In fact, with the number of sites available today, I doubt that even a unique design is really that unique.

So save yourself a lot of time and money and just go grab a design from one of the many sites that specialize in skins or themes for sites.  Even if you have to modify it a bit, you’ll be much further ahead of the game.

The flexibility that DotNetNuke gives you comes at a pretty high cost both in terms of finding Skins that work reliably right out of the box and in terms of training users to use the site.  So just because DNN is more flexible does not necessarily mean it is “better.”

## The Questions I Ask

What are the requirements of the site?

If I have a requirement that can only be met by one of the platforms, that pretty much ends the list of questions.  If I can meet all the requirements with WordPress, that will probably be my choice of platform unless a future question indicates other wise.  My second choice would be DotNetNuke.

Is there any possibility of a future requirement that would lead me to another platform?

We want to try our best not to lock ourselves out of expansion.  At the same time we don’t want to provide more room for growth than will ever be needed.

Will the requirements for this project require me to write special code?

If I can’t find a plug-in or module that does what I need it to do, then I’m going to automatically be left with two choices.  Write it myself or use DotNetNuke and write a module.

Does the client want to be able to maintain the site himself?

If the answer to this is “yes” I’m going to lean very heavily toward WordPress as my first choice and DotNetNuke as my second.

What servers are available to install this on?

If you don’t have a Linux server, you are kind of stuck.  This isn’t true in my case, but for some of my clients, it is a factor.  It doesn’t completely rule out using WordPress but it does discourage from going in that direction.

Is Search Engine Optimization a Factor?

For most people it is.  Again, this favors WordPress.

## When Would You Just Write It Yourself?

At this point, hardly ever.  Sometimes the choice is made for you. “I want you to write this from scratch using ASP.NET.”  Well, OK, if you're willing to pay for it…

But it really seems silly to have to write code I know I can get for next to nothing.

You may not have the same selection of tools that I do.  I wish I had some time to look at a few others.  But you should be asking similar questions about the project at hand as you decide how you are going to achieve the goals of the project.  “If all you have is a hammer, everything looks like a nail.”
