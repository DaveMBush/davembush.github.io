---
title: The point of a multi layer architecture
tags:
  - architecture
  - asp.net
  - windows
url: 90.html
id: 90
categories:
  - ASP.NET
date: 2008-01-31 08:19:54
---

I'm planning on doing a series of videos on multi-layer/multi-tiered architecture using LINQ, but before I get to that I thought it would be helpful to first discuss the advantages of multi-layered architectures. If we don't understand what it is we are trying to achieve, we'll never know if the solution is the proper solution. A multi-layered architecture generally looks something like this:

[![image](/uploads/2008/01/image-thumb.png)](/uploads/2008/01/image.png)

There is a forth component to layering which is the actual data, normally in the form of a database, that is called by the Data Access Layer (DAL) but normally the DAL and the actual data are pretty tightly coupled.  While there is a theoretical possibility that the actual data source could be swapped out, the reality is that the swapping amounts to little more than changing a connection so that the DAL is retrieving different data but it is exactly the same format.  So, for purposes of this discussion we will assume the DAL and the data are actually one and the same.

With that out of the way, what we have is a presentation layer that could be a web page, a windows application, a web service, or even a telephone.  The only thing that should be in the presentation layer is stuff that is strictly needed needed to get the data in front of the user.

An easy way to determine if the code you are writing belongs in the presentation layer or not is this.  Pick some other user interface.  If you are writing for Windows, think Web.  If you are writing for the Web, think Windows.  Then ask this question.  "If I had to rewrite the user interface in the other platform, would I need to duplicate this code?"  If the answer is yes, you probably want the code in the Busines Logic Layer (BLL) not the presentation layer.

We are going to skip over the Business Logic Layer for a second and get right to the DAL.  The DAL should only have code in it that gets data out of the database and puts data back into it.  That's it.  Basic CRUD.  This is not to say that you might not have a join or a complex stored procedure that collects data from multiple tables before it returns the data up to the DAL or the BLL.  My general rule of thumb here is, if it only relates to the database, do it in the database and/or the DAL.  What you shouldn't be doing is retrieving data from the database that is user interface specific.  For example, if you have a drop down list that you need to display "Select One of the Following" as one of the "options,"  "Select One of the Following" should not be text that is in the database.  That's presentation information.

Maybe that sounds obvious, but I've seen it done.

If you write the DAL/Database code correctly, you should be able to move the DAL/Database to another application and use the same code in it without a lot of trouble.  You should also be able to switch out databases in your project by simply rewriting or even just modifying the DAL.  I have one project I'm working on now where the data is being stored in MS-Access.  I'm writing the DAL using SELECT, INSERT, and UPDATE statements in the DAL.  But, when this project moves to SQL, I'll take those same statements and make them stored procedures and have the DAL call the stored procedures.  My other code will remain exactly as it is being written now.

So, what's the BLL do?  It's the glue that gets the data to the presentation layer and the changes that we make to the data in the presentation layer back down to the DAL.  About 80% of the time all it does is pass data back and forth.

It's the other 20% of the time that makes the BLL critical to a good design.  In other words, most of the time you could write your code by bypassing the BLL completely.  However, there are two important, and sometimes critical functions that the BLL provides.

First, the BLL provides the ability to validate our data as we send it down to the DAL.  We can test for conditions such as length, value ranges, and other conditions that we need to check prior to putting the data into the database.  Obviously, this should have all been checked at the presentation layer.  But, this is a last chance to ensure our data is clean.

The second reason for providing a BLL in your code is because many times the data that we need to display is slightly different than the data we need to store.  I have a few projects where, what I'm displaying is a check box.  But, what I'm storing is a character, or a series of characters.  I use the BLL as a kind of translator.

This second instance is a bit tricky because I've already said we want to keep presentation layer things in the presentation layer.  Strictly speaking, this translation stuff I'm talking about is more presentation than business logic.  But, it has to be done some place and I've found that the BLL is the best place.  If you find that you need a translator of this kind that is user interface specific, you might consider providing another layer between the presentation layer and the BLL.  Since I don't know of anyone that is providing a standard name for it now we'll just call it the UIBLL.

I have one of these types of situations now where the code I'm working on is being stored to a database that will ultimately be rendered to a web site.  The text box on the web site wants the carriage return / linefeed pair but the text box in a windows application only wants the carriage return.  So, I need a kind of translator to make sure I'm viewing the data correctly in windows and storing it back into the database so that the web site can display that same data as expected.

I've seen many people frustrated by the databinding functionality that Microsoft provides because they try to do this type of translation in the presentation layer.  By placing the translation down a layer, databinding actually makes a lot more sense.

Finally, if you've done the layering correctly, you should be able to place these layers on different computers and have everything still work correctly.  You may wrap the DAL in a web service and have the BLL call it from another computer.  You could then have the BLL wrapped as a web service and have either a web application on another computer or a windows application on another computer call it.  You could even have a hand held device (Pocket PC or Windows Mobile anyone?) call it.

By layering your application, you get the ultimate in flexibility, the ultimate in code reuse, and solve some pretty common programming scenarios almost for free.
