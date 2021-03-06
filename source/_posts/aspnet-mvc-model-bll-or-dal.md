---
title: ASP.NET MVC - Model != BLL or DAL
tags:
  - archietcture
  - bll
  - dal
  - MVC
  - Three Tiered
  - view
url: 809.html
id: 809
categories:
  - ASP.NET MVC
date: 2009-02-11 05:36:37
---

Last week I introduced the ASP.NET MVC framework by talking a bit about what the model, view and controller are. In the comments, John Meyer said,

> I respectfully disagree with your claim that the model is your BLL. MVC is a UI layer pattern, and as such all models, views, and controllers are strictly in the UI level.

While historically, MVC has been described in the way I stated--while the ASP.NET MVC guys have also portrayed the Model as BLL or below--I have to agree with John.  Here's why: At least as far as ASP.NET is concerned, the model is inherited from a specific class.  This means that any implementation code you place in the class will be forever tied to the class it inherits from.

<!-- more -->

So if in some point in the future you decide that a WebForms implementation would work out better for you, or you wanted to put a Windows Forms implementation on top of it, you'd have to do quite a bit of refactoring of your code just so you could.

If instead you treat the Model as a "View Model" as John suggests, and have the View Model call the Business Logic Layer, you end up with two major benefits.

First, your Business Logic Layer is completely decoupled from the View implementation.  You are no longer forever tied to MVC as an architecture or ASP.NET MVC as the primary architecture.  You can use whatever view implementation you want.

Second, you are not forced to put View specific data code in your Business Logic Layer.  Doing so would cloud the actual implementation of your BLL and actually further couple your view layer to your BLL, something that third tier is specifically designed to avoid.

Based on the feedback from John and my own thinking on the subject, I recommend a three-tiered approach that places the MVC as the view entity calling the BLL from the Model of the MVC set, which would in turn call the Data Access layer.
