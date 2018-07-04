---
title: ASP.NET MVC - Controller to View
tags:
  - asp.net
  - controller
  - MVC
  - view
url: 980.html
id: 980
categories:
  - ASP.NET MVC
date: 2013-10-30 15:04:38
---

![ka_vol1_100](/uploads/2009/04/ka-vol1-100.jpg) A couple of weeks ago we looked at ASP.NET MVC routing in the MVC framework.  The routing controls which method in which controller gets called.

The obvious next question is, how do we get from the controller to the view?

First, we need to look at the general layout of our Views.

If you open up the sample project that we created, you'll see that there are a few directories that have been created.  The one we want to take a look at today is the View directory.

You'll see that under each View directory is a directory that has the same name as each of the controllers in the Controller directory as well as a directory named 'Shared' that has nothing to do with MVC directly.  Don't worry about figuring that one out right now.

Under each of the directories that map to the controller, you'll see that there is an ASPX file that maps to each of the methods in the controllers, or is otherwise called from those controllers.

The easiest way to get from the Controller action to the View it corresponds to is to return View() from that action, as in:

public ActionResult About()
{
    return View();
}

[](//11011.net/software/vspaste)This would then call ~/Home/About

But what if the new page needs to have data sent along to it?

In this case, you can assign the data to the ViewData property.  The ViewData property works a lot like a Session object in that it is keyed.

So, to pass data you would use

ViewData\["Key"\] = objectData;

[](//11011.net/software/vspaste)You can see that the sample project does this in the HomeController.Index method.

public ActionResult Index()
{
    ViewData\["Message"\] = "Welcome to ASP.NET MVC!";
    return View();
}

[](//11011.net/software/vspaste)And you can see that the Index.aspx file picks it up later:

    <h2>**_<%_****_= Html.Encode(ViewData\["Message"\])_** **_%>_**</h2>
    <p> To learn more about ASP.NET MVC visit 
        <a href="http://asp.net/mvc" title="ASP.NET MVC Website"> http://asp.net/mvc</a>.
    </p> 

Returning View() is not the only way of specifying the View we want to display from the Controller.  You can also return Redirect(), RedirectAction(), RedirectToRoute().
