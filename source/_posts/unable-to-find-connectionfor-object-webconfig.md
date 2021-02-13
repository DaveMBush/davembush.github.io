---
title: Unable to find connection...for object web.config
tags:
  - connection string
  - dataset
  - error
  - web.config
url: 921.html
id: 921
categories:
  - ASP.NET
date: 2009-03-18 04:40:09
---

![J01C0089](/uploads/2009/03/j01c0089.jpg) I've seen this error a couple of different times.  Mostly from other people calling me with the problem.  So I still don't know what ultimately causes the problem.  But if you're having this problem, here's how you fix it.

First, a bit of background.

You'll open a dataset you've been working with for months and see the following error message:

> Unable to find connection \[connectionName\] (web.config)1 for object 'Web.config'.  The connection string could not be found in application settings, or the data provider associated with the connection string could not be loaded.

What has happened is that the list of connection strings the tables in your dataset use has become corrupt.  In the most recent case of this we ended up with two connection strings with the same name pointing to two different connections. Here's what you need to do to start trouble-shooting the problem.

*   Close the dataset window with the error.
*   Right click the dataset in the solution explorer
*   Select "Open with..." from the context menu
*   Select "Source Code (Text) Editor" from the list and press the "OK" button.

Your code will now be opened in text mode so you can view the XML.  You want to be careful at this point because if you mess things up too badly, you won't have anything useful.  As things stand now there is hope of recovery.  A backup would be a good thing to make sure you have right about now. Near the top of the file, you'll see a \<connections\> element with several \<connection\> elements within it.  Most datasets should only have one \<connection\> element.  If you are having trouble like I've described above, you'll have at least two and one will be incorrect.  Delete it or otherwise fix it, save the file and reopen normally.  If you are lucky, everything will be working correctly again.
