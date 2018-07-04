---
title: WebForms vs MVC–The War Is Over
tags:
  - asp.net
  - css
  - html
  - javascript
  - MVC
  - webforms
url: 2644.html
id: 2644
categories:
  - ASP.NET MVC
  - Javascript
date: 2014-09-25 06:00:00
---

loading...

I just finished listening to a [DotNetRocks podcast today with Paul Sheriff](//www.dotnetrocks.com/default.aspx?showNum=1033) which largely talked about creating mobile web sites using ASP.NET WebForms.

During the show they discuss when you might use WebForms vs MVC and pointed out that WebForms still have their place, particularly in the corporate world, since they are just as testable as MVC and yet much easier to get something up and running quickly.  This is in contrast to a public facing site that might need to be as light weight as possible and therefore need the extra control that MVC can give you.

And then there is the issue of legacy code.  As they mentioned in the podcast, just because Microsoft has introduced a new technology, doesn’t mean we all have to go re-implement our websites in that technology the next day.  It doesn’t even mean we have to ever implement that technology.  It is just another option in our toolbox.  It is our job as developers and architects to know when the right time to use what technology is based on the requirements of the application and the skill set of our development team.

[These are Points I’ve made in the past.](/aspnet-mvc-is-the-grass-really-greener/)

The War Is Over
---------------

The point I want to make today is this.  The war is over already.  I am surprised that this is still a conversation that the community is having.  Both sides have “lost” to a sneak attack by SPAs.  If this is still a conversation you think is worth having, I would suggest that you may be behind the learning curve.  The .NET world has moved on.

That is, client side models and frameworks have taken over.  What you use to produce the HTML that all lives in, is of very minor concern at best.

I suppose if you are interested in creating applications that are old style forms that post content to the server and refresh the page content or move on to another page, you might be interested in continuing to fight this war.  But in my world, most of the applications I work on could be created using HTML, CSS, and JavaScript calling back to a server that consumes and produces JSON.  In .NET, that would mean using WebAPI, which, while based on MVC, isn’t MVC.

Examples
--------

For some examples of this, check out the LabelsForEducation site which is a DotNetNuke site.  DotNetNuke is a CMS based on WebForms technology.  By using jQuery and knockout, I was able to create the interactive portions of the site without using any webforms technology directly.  Instead, I just produced enough HTML and JavaScript in the ASCX file that is the modules I developed to launch the main JavaScript application. You can check out how this looks on the following pages:

*   [Find Your School](//www.labelsforeducation.com/My-School/Add-a-School?PostalCode=06410)
*   [Participating Products](//www.labelsforeducation.com/Earn-Points/Participating-Products)
*   The “UPC Lookup” tab on the Participating Products page above
*   Much of the Signup logic on the [LabelsForEducation](//www.labelsforeducation.com/) web site

You would be surprised at how very little code is in the codebehind files of my ASCX files that each of my modules live in.

In my current project, I’m using EXTjs and ExtDirect, which is based on WebAPI, to create SPAs.  Nothing that I am currently doing requires that I do the work in either WebForms or MVC.

The beauty of an SPA is that it never officially post back so it doesn’t matter what you are using to hold the main application.

Drawbacks to SPAs
-----------------

Having said all of that, there is a major drawback to SPAs.  You have to really know JavaScript, and I’m guessing even though it is the most used language, the number of people who really know JavaScript relative to the number of people it would take to implement a good SPA in a way that can be tested and maintained is relatively few.

Now, I’m sure someone will argue that we still need something.  In fact, as I was discussing this article with some of my peers, we recognized that WebForms and MVC at least allow us to provide page level security.  But even that could be handled using a regular HTML page either by applying the security at the IIS level or by implementing security in the WebAPI and JavaScript.

And what if your page has no interactive components?  Or just a part of the page has some interactive components?  Maybe the bulk of the content comes from a database.  In that case, you can use either and neither  would necessarily be “bad.”

What About Database Driven Static Sites?
----------------------------------------

The part that is posting back can be implemented using JavaScript and WebAPI.  The rest can be implemented by turning off state management on WebForms and using databinding and Model View Presenter(MVP) or Model View ViewModel (MVVM).  I prefer MVP. Or you can go for MVC and Razor.  Either will essentially get you where you need to go and be relatively light weight.  Although one could make the argument that MVC will give you more control.  The question you have to ask though is, if you had the control, would you really use it.  Does the performance gain relative to the traffic you expect to get and the level of experience your developers have make the switch to MVC worth it?  Would it make more sense to concentrate training efforts on improving JavaScript and WebAPI skills?  Just some things to think about.

So, stop arguing about WebForms vs MVC and put all of that energy into REALLY learning JavaScript, CSS, and HTML because that is where you will be spending a huge hunk of your time in the future as a web application programmer.

The Internet has moved on.  HTML, CSS and JavaScript aren’t just designer eye candy toys anymore.  They are first class development tools that you need to learn now if you plan on being marketable 5 to 10 years from now.

#### Other Places Talking About WebForms vs MVC

*   [Mixing Web Forms and ASP.NET MVC](//www.simple-talk.com/dotnet/asp.net/mixing-web-forms-and-asp.net-mvc/)
*   [.NET Web Forms vs. MVC: Which is Better?](//www.seguetech.com/blog/2013/12/05/dotnet-web-forms-vs-mvc-which-better)
*   [Web Forms, MVC, Single Page App, Comparison](//prasadhonrao.com/web-forms-mvc-single-page-app-web-pages-comparison/)
