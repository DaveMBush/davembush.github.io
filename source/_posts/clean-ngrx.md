---
title: Clean State Management with NgRx
date: 2022-07-04 12:46:07
tags:
---

Once again I've run into a situation where the code I'm looking at doesn't resemble how the code was meant to be written.

I've spent the last month fixing bugs that can all be summarized by the following problems:

- Reducers updating nested data.
- Storing data manipulations that Selectors could perform
- Using Effects as Selectors
- Components doing too much data manipulation

<!-- more -->

Unfortunately, the people who need to hear this the most are the ones who will never read this article. Some of what I'm about to say, I may have discovered over the last several years of programming but I'm sure I didn't discover this in isolation.

So, lets review how NgRX should be used effectively.

## The Problem with Nested Data

If you've ever worked on a system with nested state data as part of one slice, you know that updating that nested data is a pain. For those of you who haven't, having to update that data makes your reducer 10 times longer and 3 times more complicated than it needs to be.

This means, first, you've written more code than you need to so there is a higher possibility of errors.  But beyond that, the code your customer has to download is also larger than it needs to be.  Worse, because it runs every time the nested data is updated, the code performs slower than it needs to.

## The Problem with Storing State the Way You Want to Use It

Related to nested data is the urge to store state the way you want to use it later on in the application.

The main problem with doing this is that you need to make sure that state gets updated correctly from every place you update the raw data. In the process, you will invariably run that update the derived state code multiple times when you could have only run it once.

Instead, you should modify the data into something your presentation needs using Selectors. Since everyone should be using the selectors to get the information they need, and that selector is looking at the raw data, you can be sure that everyone is getting the correct data.

You have the advantage of taking advantage of memoization so that this data manipulation only happens when the data is needed and when it changes.

## Storing State

Rule number one of state management is to store state at the most granular level you can. This means, in part, that each slice of your state, the table in your store, should be flat.

What does it mean that the data is flat? It means that the object only contains primitive values. Strings, number, booleans, dates, or collections of objects that only contain primitive values. Just like you would in a relational database table.

But, this may not be the most granular level.

Say you have a row in your database that contains 3 fields that you always want but 50 or so that you only want if a particular presentation is running.  I would recommend creating a slice for the 3 main fields and then another slice or more, for the remaining fields and allow for the fact that you may not have all of them all the time.

While most systems don't need this. The system I'm working on does.

This solves the storing nested data issue.

## The Job of Effects

Apparently, some people think that the job of an effect is to manipulate data.  But as we've already established, manipulating data is the job of selectors. No, the job of Effects is to retrieve data from wherever we are persisting it and to update the persistent store with any changes we've made. If we need to manipulate data to do either of those, again, Selectors is where we would do this work.

Once you've narrowed the job of your effects down to this, you no longer need to return multiple actions from an effect.  Ideally and effect should fire, at most one action.  But, no more than two.

## The Job of Reducers

Reducers only have three jobs: add new data to the slice, remove data from the slice, and update data in the slice. That's it. If your reducers are doing more than that, you're doing it wrong.

## Extra Credit

The above, is the bare minimum and may server you well for small projects. However, for larger projects you will want to consider using NgRX Entities.  This will reduce the amount of boiler plate code you need to write as well as making it easy to join your slices however you need to.

One place in the code I'm working on this would be particularly useful is to see what slice owns another slice.  As the data is stored and returned we only know who the children are most of the time.  To find the parent, we have to iterate through the data to find the child and then look at what parent we are in.

It gets even more complicated when a row of data could have more than one parent.

## Thinking About Code

The last point I want to make regarding state management is that how we think about our code impacts how we write our code.

I've discovered over the years that most of us think about code from a GUI perspective. But, by doing this, we also tend to put a lot of logic in our components and keep doing so, until that doesn't work any more. At that point, we start to consider state management.

But what if we thought about code strictly from a data perspective? If every time you thought about the code you were writing and though, "what if I didn't have a presentation layer? What problem am I trying to solve really? How can I do this without a presentation layer?" Then, your code would be more flexible, easier to maintain, and more scalable.

How would it be more flexible?

Back as the dotCom days were starting I had a manager come to me with a VB6 😝 application he had had build and said, "I'd like to take this code and run it on the web, or from a phone, or from the desktop."  And my response to him was, "You can't get their from here." because the code was so tightly coupled to the presentation layer.

But to bring it today, we often have the same kind of requirements. We originally solve a problem only to have our managers come to us with a new requirement that suggest using the same data to look at the code in a different way.  If all your data logic is in the presentation layer, you might achieve the goal but you'll also have a lot of duplicate code if you are very careful. But, lets say you manage to pull that off. Six months later you are working on a bug in one of those two views. You fix it, but you forget about that other view and never think to fix it there too. And this is where your troubles begin.

By pushing code as far down the stack as is practical, you reduce the amount of code you write, reduce the size of the code that your customer has to download, reduce the code you have to maintain, and in the case of NgRX end up writing code that performs faster.

So, how might you force yourself to think in this way? By following this simple rule: Presentational components should only 1) display the data they've been give or 2) fire events indicating that some action has taken place.

There is another type of component generally referred to as "Smart" components. They too have a rule. They should only 1) retrieve data from the store via Selectors and 2) fire actions to update the Store or trigger an Effect.
