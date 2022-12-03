---
title: Using Workers from an Angular Library
date: 2022-12-03 15:21:48
tags:
  - angular
  - web-workers
  - libraries
---

This past week several of us have been working hard to implement a SharedWorker from a library. We found a number of articles on the web that demonstrated how to get Web Workers setup for Angular that all assumed the Web Worker was being generated in the main Angular app. We wanted to use a library and include that file in the main app.

<!-- more -->

If that's what you are trying to do, I hope this post saves you the time we spent this week figuring it out.

## The Problem

As all the article state, you need to use a generator to create the Web Worker files and modify your configs. The Angular documentation gives you the steps here: [https://angular.io/guide/web-worker](https://angular.io/guide/web-worker).

What isn't clear from this documentation is that this ONLY works for apps. If you specify a library, the compiler doesn't know what to do with it.

## The Solution

The solution is relatively simple. Create the worker files in the library. In the library put a file that implements the worker. In the worker file that the schematic generated in your app, import the worker file from the library.

Everything else stays the same as is documented elsewhere.
