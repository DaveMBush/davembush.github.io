---
title: Are You Thinking Clearly About Your Architectural Choices?
tags:
  - architecture
  - programming
  - testing
url: 3231.html
id: 3231
categories:
  - Software Architecture
date: 2015-09-17 07:30:00
---

Recently someone asked me where the business rules should go in an MVC framework.  The Model or the Controller? This reminded me of a post I wrote when ASP.NET MVC was first released.

*   [ASP.NET MVC – Model != BLL or DAL](/aspnet-mvc-model-bll-or-dal/)

But today I want to cover a broader topic common to everyone, not just programmers.  Not being able to think outside the box.

![image](/uploads/2015/09/image2.png "image")

<!-- more -->

Here’s the deal.
----------------

Good salesmen have known for years that the best way to close the sale is to ask the customer to pick between two buying decisions.  You never ask, “So, did you want to buy that today?”  Because almost invariably the customer will say, “No” because the implication is, “Did you want to buy that today, or some other day?”  And “some other day” is always a better option to the customer.  No, the proper way to frame the question is, “So, how did you want to pay for that?  Cash or Credit?” or “Did you want the blue one or the green one?”  By doing this, the customer is no longer even thinking “later” is an option.  The only options he is thinking about are the options he’s been presented with.  This isn’t to say that there isn’t a customer who can fight past this and respond, “No, I was just looking today.”  But the chance of the customer leaving without buying have just been reduced dramatically because two buying choices have been presented.

So, what does this have to do with programming?  Well, that’s exactly what happens when we’ve been given an architectural framework to program with.

Someone shows you the MVC pattern and you immediately think that the only places code can live in our application are in the Model, the View, or the Controller.

Someone shows you the MVP pattern and you think code can only live in the Model, the View or the Presenter.

Someone shows you the MVVM pattern and we think code only lives in the Model, the View or the ViewModel.

And so, I ask you. Given any of those three patterns, where do you put your business logic?  And while we are at it, where does your data access logic go?

Do we place it in the model?
----------------------------

Well, what is the model for?  A model is for storing data.  For most of us the data is a record that is displayed on the screen as a series of fields.  But how it gets displayed and what it actually contains doesn’t matter as much as the fact that the model **stores data**.  Nothing there even hints at executing code.  In fact, I would argue that for most of our applications, the model should be so simple that it doesn’t need to be tested.

How about the Controller or the Presenter?
------------------------------------------

A controller or presenter sends commands to the model to change its state and sends commands to the view to update the presentation.  Here things get a little confusing.  It sounds like, because the controller is sending commands the the model and the view that it is where the business rules live.

In fact, when I was first introduced to MVC, this is what I thought was true.  But notice there is nothing in the statement about what a controller does that would indicate that it is anything more than a traffic cop.

View Model?
-----------

In the MVVM pattern, it becomes even clearer that there is no clear place for business logic because the ViewModel holds the state of the view.  The presenter or controller part is handled by a binder that is typically part of the framework you are using, as with KnockOut.

Thinking outside the box
------------------------

Now that we’ve demonstrated that none of these frameworks explicitly state where the business logic should go, where should we put it? In my case, what I’ve started doing is that I’ve created a service layer, or a business rules layer.  It handles the processing of the logic to get the rest of the code working.  The classes in this layer can either be passed the model or viewmodel in which they can change the state directly, or they can return data that the controller can distribute as needed.  In my most recent application I selected the pass the model in approach.  The result was code that was much more testable than what I had started with, which is the whole point of placing the business rules outside of the pattern to begin with.  

### Resources

*   [Model View Controller](//en.wikipedia.org/wiki/Model–view–controller)
*   [Model View Presenter](//en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93presenter)
*   [Model View ViewModel](//en.wikipedia.org/wiki/Model_View_ViewModel)
