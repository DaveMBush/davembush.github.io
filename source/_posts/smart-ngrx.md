---
title: smart-ngrx
date: 2024-07-07 13:36:27
tags:
  - ngrx
  - angular
---

Imagine an NgRX world where you almost never knew you were using NgRX. A world where you never had to write a `reducer`, `action`, or `effect`. A world where you never had to call `store.dispatch()`. A world where the data you worked automatically persist to the server. A world where the data is retrieved from the server as it is needed and removed from memory when it is not, or never removed if that's what you want. A world where the data automatically refreshes from the server, or you can use websocket messages to refresh the data and, in either case, the only data that refreshes is the data the code is actively using. A world where optimistic UI is built into the framework.

<!-- more -->

Introducing [SmartNgRX](https://www.npmjs.com/package/@smarttools/smart-ngrx). A framework I've been working on for over a year that does everything I just mentioned and more. It's a framework that is built on top of NgRX and works with your existing NgRX code.

The full documentation is available at [SmartNgRX Documentation](https://davembush.github.io/SmartNgRX/home) but let me give you a brief overview of how it works.

## Brief Overview

First, there are two providers you will need to add to your application. The first is `provideSmartNgRX()` which is added to the `providers` array in you `AppModule`. The second is `provideSmartNgRX()` which is added to the module, or route, nearest where you'll use it.

These two providers setup the configuration information that SmartNgRX will need to work. They control things such as how often to refresh the data, when to remove unused data from memory, what service to call to retrieve the data for an NgRX slice looks like and what a placeholder row looks like for a particular slice of data.

In order for you to define the service an the SmartNgRX effect will call, you'll need to create an EffectService. This service is where you control how the CRUD operations interact with the server. This and the selectors are the only code you'll need to write.

Which brings us to the selectors. Instead of using the `createSelector` function from NgRX, you'll use the `createSmartNgRXSelector` function from SmartNgRX. This function defines the relationship between a parent selector and any children it may have.

That's it. All the other code you normally write is handled by SmartNgRX.

Give it a try and let me know what you think.
