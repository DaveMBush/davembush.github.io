---
title: Using NgRX to Cleanly Aggregate Data
tags:
  - angular
  - design patterns
  - NgRX
url: 4455.html
id: 4455
categories:
  - Angular
date: 2017-10-03 06:30:50
---

For the last 18 months, I've been working for an organization that has what some might consider a unique requirement.  Because of where our application's data is sourced, we need to aggregate data on the client side rather than on the server.  What this means is that for any one screen, we may make multiple calls to the server to grab all the data we need.  Fortunately, because we adopted NgRX early in our adoption of Angular, we could avoid a lot of the headaches associated with client-side aggregation. <figure>![](/uploads/2017/09/2017-10-03.png "Using NgRX to Cleanly Aggregate Data")<figcaption>Photo credit: [NASA Goddard Photo and Video](//www.flickr.com/photos/gsfc/14486243743/) via [Visualhunt](//visualhunt.com/re/296da9) / [ CC BY](//creativecommons.org/licenses/by/2.0/)</figcaption></figure>

<!-- more --> 

The Problem
-----------

There are multiple ways this problem might manifest itself in an application.  But one of the most common is a basic search screen that displays a list of results.  Everything is simple when your data comes back with all the data you need.  But in our case, the data that is returned might contain all but one or two fields that we need.  Those fields exist in other end points.  To keep the basic problem small, let's just assume that you search for a list of records.  That search returns 10 items.  For each of those records, you now need to make two more calls to retrieve the content of the two missing fields.  This means that to get a complete result set back, you need to make a total of 21 calls.  The problem becomes even worse if you have a total of 100 records, or you now have 3 fields that you need to retrieve for each row.

The Old Way
-----------

Prior to using NgRX, the main way we might solve this problem would be to introduce callback hell, or promise hell if you are that lucky.

*   Make a call for the original list
*   When the list gets returned
    *   Iterate through the records and
        *   Make a call for Child Record One
            *   When callback returns, add the new value to the parent record
        *   Make a call for Child Record Two
            *   When the callback returns, add the new value to the parent record
*   Once all the calls have returned, return the list so it can be displayed.

As you can see, this not only becomes difficult to manage, but it also introduces a system that is going to be perceived as slow. But, now, we can do better.

Using NgRX
----------

By using NgRX, we use a series of Effects to retrieve our data, typically via a Service.  When the effect is done, it returns the results to a reducer which puts them in our store entity for us. The basic work flow looks like something like this:

*   Dispatch an action to get the main results
*   Effect hears the action and makes a call for the top-level list
*   When the list returns,
    *   Iterate through the records and
        *   we dispatch an action to get Child Record One
        *   we dispatch an action to get Child Record Two
*   Return an Action that will use a reducer to fill our list
*   Child Record One Effect hears the actions for each of the rows
*   When each of the values are retrieved the Effect returns an Action that uses a Reducer to put the value in the store
*   Child Record Two Effect hears the actions for each of the rows
*   When each of the values are retrieved, the Effect returns an Action that uses a Reducer to put the value in the store

You'll notice, we no longer have the nesting mess that we had using the old way and we can list our results as soon as the first set of data is returned.

Meanwhile, back on our View
---------------------------

Now, there are two ways you can deal with displaying this information in your view and it all depends on what you are doing. The easy way is to just let the View display the information as it comes back.  Most of the time this will work.  If you need to filter your data in the display once it comes back, you will need to decide if data that doesn't have the child fields yet should, or should not be displayed. Another quirk I had to deal with was that we were displaying child rows with child rows.  Letting the data display as we got it back gave the screen a kind of exploding effect.  For this, I added a debounceTime(500) to the store observer so that the screen only updated once all the data had been retrieved.  Using the pattern above was still easier to reason about than the old way, we just didn't get the added benefit of being able to see the data as it was being retrieved.

Watch Out!
----------

One of the wrong ways you might be tempted to use this pattern would be to chain all the child stuff in one effect and dispatch actions to your reducers from within the one Effect.  This would be a mistake.  Sure, it would work.  But now because your effect is doing more than one thing, your code becomes MUCH harder to reason about.  While each of your Effects may ultimately call the same reducer function, or not, you definitely want to have a separate set of Actions and Effects that retrieve the data from the server.
