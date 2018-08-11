---
title: Upgrade to Angular from...
tags:
  - angular
  - portal
  - react.js
  - web 1.0
  - web 2.0
url: 4449.html
id: 4449
categories:
  - Angular
date: 2017-09-26 06:30:23
---

As I've been interviewing for a new contract, the question, "How do we Upgrade to Angular from ...?" has come up several times.  And as I've thought about the question, several patterns have emerged. <figure>![](/uploads/2017/09/2017-09-26.jpg "Upgrade Angular from ...")<figcaption>Photo credit: [archer10 (Dennis) 102M Views](//www.flickr.com/photos/archer10/12414442945/) via [Visualhunt](//visualhunt.com/re/426124) / [ CC BY-SA](//creativecommons.org/licenses/by-sa/2.0/)</figcaption></figure>

<!-- more --> 

From a Portal
-------------

The first time I was asked this question, the team I was talking with had an existing Web 1.0 site setup using WebSphere Portal.  Not something that you can easily plug any Single Page Application (SPA) into. But all is not lost. One of the easiest ways to solve the problem is to create a reverse proxy.  One popular tool for this is mod_proxy for the Apache server.  If you are using IIS, you could use URL Rewrite rules to setup a reverse proxy. In either case, you will need to setup rules that ensure that anyone trying to access a URL that is still on the old portal site would still be able to access that directly.  Once that is in place, you would create one page, or maybe a set of related pages, at a time and stand them up on another server.  Then you would change your rules so that anyone trying to access the old URLs that are no longer being served by the old site get redirected to the new SPA application. Now, this sounds pretty simple.  But, I imagine keeping everything in sync as you go might be more of a pain that you might imagine.  If you have pages that link to each other, you'll need to convert links to routes as you go.  Say you convert page one.  It still has links to the old site.  Not a big problem.  But as the pages it is linking to now become part of the SPA, you'll want to convert those links to route directives.  One way you might mitigate this issue is to convert pages with the fewest links out first.  It might help to create a graph, or at least a spreadsheet, to help you manage the interdependent relationships. And added advantage of this setup is that any pages in the portal that have relatively static content, and any resources, can be cached, or even moved, to the proxy server where they will probably be served up faster than the original portal.

From a Web 1.0 Site
-------------------

Switching from an existing Web 1.0 site might be a bit easier.  Although, using the proxy method there still might be the best choice.  Many of the same issues exist.  But in the case of an existing Web 1.0 site that doesn't depend on a portal, you'll probably have greater control over the server.  Again, you are probably running the site on Apache, nginx, or IIS.  In all of those cases, you can setup a rule that says, "if you can't find the URL, load up this page over here instead."  Where "this page over here" is your Angular site you are converting to.  You'll need to implement something like this anyhow for your Angular SPA to work correctly, so this is probably the least amount of extra work to get you where you need to be. Then, you convert a page or a set of related pages at a time like I described in the Portal implementation.

From Another Framework
----------------------

Let's say that you are moving from React, or some other framework to Angular.  I know of a company that is doing just this.  The wrinkle is, they have no plans on converting their existing pages to Angular.  This makes the transition relatively simple.  You setup rules on your server, similar to how you would setup a conversion from a Web 1.0 site.  "URLs that look like this, go to the old index.html page and URLs that look like that go to the new index.html page." Once again, you'll need to keep track of routes as you change from one to the other.  But it is manageable.

Converting AngularJS to Angular
-------------------------------

Yes, I know ngUpgrade exist.  But, if you don't need to use it, why would you.  ngUpgrade exist so you can embed Angular in AngularJS or AngularJS in Angular.  But, it might make more sense to create a new SPA for Angular and have the two sites reference each other similar to how you would do this for a Web 1.0 site or if you were converting from an entirely different framework. The reason I recommend using the two-site method first is because Angular really is a different framework.  There are similarities between AngularJS and Angular.  But that is all they are.  What if you just pretended they weren't at all related.  That the ngUpdate bridge didn't exist.  Wouldn't that make life just a bit easier in the long run?  I think it would.

What do You Think?
------------------

What do you think?  Do you have experience converting to Angular from an older technology?  What problems did you face?  How did you solve them?  Let me know in the comments.
