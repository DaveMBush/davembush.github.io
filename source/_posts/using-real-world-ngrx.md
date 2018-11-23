---
title: Using Real World NgRX
tags:
  - angular
  - design patterns
  - NgRX
url: 4315.html
id: 4315
categories:
  - Angular
date: 2017-05-09 06:30:00
---

This week, I want to demonstrate some ways you might use NgRX in your own code.

## Review

Last week we went into a lot of detail about how the NgRX system should be wired together. Here is all of that in picture form.

![image](/uploads/2017/05/image.png "image")

A component fires an event to either an effect or a reducer using an action. If an effect was called, it fires another action which is normally picked up by a reducer. The Reducer mutates the state which then gets placed in the store. “Magic happens here.” You don’t write any code to get it to the store other than that you create a reducer. Anything that is observing the table in the store that got changed will get notified via RxJS observables and the cycle is complete.

Note that the action you dispatch can be handled by either an Effect, a Reducer or both.

## Basic CRUD

Most of the time, we think of NgRX as a way of handling CRUD operations. We need to see the current record so we fire off a LOAD action that uses an effect to retrieve the data from the database. Once the data comes back, we return an action that tells the reducer to put that new data into the store. Since our component is observing the store, it updates the screen with the new values.

If we need to add a record, we fire an ADD action. If we need to delete we fire a DELETE action. If we need to update, we fire an UPDATE action. Each of these are picked up by the Effect, which then fires an action that places new state information in the store via the Reducer.

## Wait State

When most people think of how to use NgRX or any similar pattern, they immediately think of the CRUD pattern I mentioned above. But, we don’t have to start the Action chain from a component. For that matter, we don’t have to listen to our Store data from a component either.

One way I’ve implemented NgRX that solves a lot of common issues is that I’ve created a wait state component that shows when a count variable has been incremented and doesn’t show when the count is zero. Since Store is `Injectable`, I can increment and decrement the count from just about anywhere. Most often I increment it from an effect just before I make an AJAX call and decrement it in a finally() block of the Http observable. I have a start() action that increments the count and an end() action that decrements the count and ensures that I never go below zero. I can start off multiple asynchronous processes which will all increment the counter and decrement the counter appropriately. The wait state GUI displays until everything has finished.

This is OH! So much easier than how we’ve had to handle this problem with other design patterns. I’m not saying it couldn’t be done, or that it was even particularly hard. But this way is easier.

## Error Handling

Another place where you might want to present information but trigger the display from just about anywhere is with error handling. In the code I work on, I have a modal popup component that displays whenever my error collection has something in it. Anytime I need to display an error, I add the error to the collection via an Action and it magically displays. The great thing about this mechanism is that regardless of how many errors I send to the collection, they all display until I close the window, which clears out the collection.

## Page State

Most page state is handled by the fact that we’ve stored the data into a database. But there are times when we want to come back to a page we had been working on previously and we want it to display with the data that was on it at the time we left.

Or maybe you want to work on a series of pages prior to saving so that everything gets saved as a set.

No matter. You can use NgRX to store everything into the store and a separate action can trigger an effect that pushes that data to the database.

Or, as is the case in an application I’m working on, I’m using a form to search a database. When I come back, I want the same search fields and I want the search to reinitialized. In my particular case, I don’t have a search field. You change a field, a new search is automatically initiated. This case is just a little bit more complicated than what we’ve looked at so far.

![image](/uploads/2017/05/image-1.png "image")

In order to keep the state information available so that it is there when I come back to it, I need to store that information in the search table via the Search Reducer. So, every time something changes in the Search Form, I send off an action to the Search Reducer so that the change can be recorded.

Meanwhile, the Search Form also is listening to the Search Table so that when it comes back it can put the changes in the form, and it can send an action to the Search Results Reducer telling it to search for the information. When it gets the results, the Search Results Table picks them up and since the Search Results Component is listening to the Search Results Table, they display.

If I leave the page, the Search Form grabs the current search parameters from the Search Table and Fills the Form and sends the action to the Search Results Reducer and the page is back where we left it.

## Conclusion

The point is, NgRX isn’t just about basic CRUD forms and because there are multiple ways you can mix and match the parts, even your basic CRUD implementation has a lot more flexibility than you might be used to.
