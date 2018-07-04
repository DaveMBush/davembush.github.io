---
title: Do This To Increase Your Client Side Web Development Speed
tags:
  - client side
  - development
  - javascript
url: 3883.html
id: 3883
categories:
  - Javascript
date: 2016-04-21 07:30:00
---

Over the last year, in particular, I’ve developed a technique for developing the client side of a web application using JavaScript, HTML and CSS that has significantly improved my development speed.  Once I tell you, it will be obvious.  At least, it is to me … now.  But as obvious as it is, I rarely see this technique used on any of the applications my peers are working on. And while this technique is an outgrowth of my involvement with TDD, this particular technique can be used with or without TDD. <figure>![](/uploads/2016/04/image-3.png "Do This To Increase Your Client Side Web Development Speed")<figcaption>Photo credit: [JD Hancock](//www.flickr.com/photos/jdhancock/4367347854/) via [VisualHunt](//visualhunt.com) / [CC BY](//creativecommons.org/licenses/by/2.0/)</figcaption></figure>

<!-- more --> 

Presentation First
------------------

For most of my career, I’ve always like to develop applications from the top down.  That is, get the GUI up first.  Don’t make it work.  Just get a picture up that you can show to the client. This isn’t the “trick” I have in mind for improving your speed.  But if you aren’t already doing this, the trick won’t help you much. The reason “Presentation First” is so important is because of a simple truth I’ve come to realize over the years.  “No one really knows what they want until they see it.”  This is particularly true in software development because, try as we have over the last several decades, we still have not come up with a way of showing in pictures what it is we are going to build. The other reason developing the GUI first is important is because it gives the client a sense that you are making rapid development.  It is amazing how much time you’ll have without interruption when you’ve been able to show something that looks really close to the real thing.

Attaching Data
--------------

The next step in my development cycle typically involves attaching data to the presentation.  Most of the programmers I know do this by making whatever REST call they need to make to the server and displaying whatever they get back.  There are at least three problems with this. First, as the data in your database changes, what displays in your presentation changes.  If you are working with a local database that you have complete control over, that is a minor problem.  But if you are, as so many of my clients are, using a common development database, you can’t be sure that the data you are playing with won’t change. Second, you really can’t be sure that what you are displaying is actually the correct fields unless you go into your database and change the contents of the fields to represent what the fields contain so that you can display that on your data. Finally, developing in this way is going to be much slower than it needs to be.

The Trick
---------

Here’s the deal.  The way most of us develop applications, once we get to the part where we need to attach some data, we attach it directly to the REST end point.  At the very least, this slows our development time down because every time we load the app to make sure it is doing what we had in mind, we have to wait for it to load the data from the server as well. But aside from the other problems I mentioned above, it also means that we are dependent on whoever is developing those other end points.  If it is you, it isn’t so bad.  But lately, I’ve been working on projects where the only thing I’ve been responsible for is developing the client side.  In my last contract, I was brought in to build an application that was dependent on data I had no control over.  I did write a little C# code to get the data from a web service into a form my JavaScript client side code could use.  But most of my time was spent writing code on the client side. But it didn’t matter, because I mocked out the data on the client side until the server side data was ready. In fact, I was able to work ahead of the guy who was providing the data BECAUSE I was mocking out the data.  The project manager spent most of that contract worried that I would end up stalled because the data wasn’t ready and I spent the same amount of time assuring her that I wouldn’t get stalled until the ONLY thing I had left to do was hook the application up to real data. At my current contract, I’m working on a team that uses Java for the back end.  How’s that?  A .NET guy working on a Java team?  Well, you see, it doesn’t matter.  I was brought in as a JavaScript specialist.  And my development methodology is the same.  I am using mock data.  When I am finally ready to see it all working live, I’ll call the REST end points that I need. But in this particular Java environment, there is an additional hurdle.  If I were to develop this application the way most of my peers are developing, I would need to build the application into JAR, WAR and EAR files and copy those files over to the JBoss server.  While I have created a script for this using Node JS and NPM, the time this takes is a bit excessive. So, instead, what I’ve done is I’ve created a separate project for the bits I’m working on.  I’ve setup an Express server using Node JS and installed proxy middleware as my last route handler.  It is setup to point to the JBoss server for anything it can’t find under Node.  So, any files I need that have already been built, I can grab from JBoss, while all of my work runs under Express. (Yes, I know Eclipse will auto deploy relatively quickly, but from what I’ve seen of the JavaScript editor, I’m not a fan of Eclipse.  Actually, from what I’ve seen of Eclipse, I’m not a fan.) What this means is that I can click the run button to fire up my Express server and it loads my local JavaScript and any stuff I need from JBoss via the proxy.  I never have to deploy my work.  And I only need to wire in the real data once I have everything else working. I have the added advantage of being able to switch between my local mock data and the real data simply by loading my local data when I want mocks or the remote data when I want the real deal.  Kind of like you might do with a DI container in .NET only on the client side.

TDD Becomes Trivial
-------------------

And now that I’m working with known, mock data, implementing Test Driven Development is trivial.  Well, for me it is trivial.  You still need to make sure you write testable code.  But at least you don’t have to worry about the data part of TDD.
