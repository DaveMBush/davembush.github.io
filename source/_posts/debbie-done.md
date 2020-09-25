---
title: “Debbie-Done”
tags:
  - agile
  - programming
  - tdd
  - testing
url: 2441.html
id: 2441
categories:
  - Testing
date: 2014-03-04 05:38:00
---

![88Tr](/uploads/2014/02/88Tr.png "88Tr")

A long long time ago, in what seems now like another world, I worked for a company as a [Clipper programmer](//en.wikipedia.org/wiki/Clipper_(programming_language)).  While I was there I heard this story about a lady named Debbie.

I was told that Debbie was a programmer who used to work for this company.  Debbie was a lazy programmer.  She worked harder at avoiding work than if she just did the job she was supposed to do.

<!-- more -->

The ultimate lazy programmer
----------------------------

For example.  Once my boss had stopped by her desk to see how she was progressing on a report she was supposed to be writing:

Debbie: Oh, that’s done.  Here.  Take a look.

The boss looked it over and found an error.  Some of the numbers didn’t match up.

Debbie: Oh, I know what that is.  I can get that fixed right away.

Which she did.  At least that’s what she made everyone believe.

After she left (shortly after this) they found out that she hadn’t even connected to a database to create that report.  The whole report was hard coded.  Every time you ran it, it gave you the same numbers.

“Debbie-Done”
-------------

The one thing I was told that sticks in my mind the most is that Debbie considered a project “done” if it compiled in linked.  She almost never ran the code.  Or if she did, she certainly didn’t run enough of it, or run it more than to make sure it didn’t crash.  Anyhow, the perception she left is that she only compiled and linked the code.  Today, we’d say all she did was build the project or solution.

But recently, I’ve discovered that many programmers work at the “Debbie-Done” level more than we’d like to admit.

I’ve always thought that programmers wrote code like I do:

*   Write a bit of code. 
*   Build the code.
*   Run the code to see if that change works as expected. 
*   Write a bit more code. 
*   Build the code.
*   Run the code to see if that change works as expected. 
*   Rinse, lather, repeat.

But what I’m discovering is that MANY programmers do not program that way at all.  No, many of them look like some version of “Debbie-Done” programming:

*   Write some code. 
*   Build the project to make sure it will build
*   Write some more code
*   Build the project
*   Rinse, lather, repeat
*   Run the code and “test” all of the changes at once.

The problem with this method is that no one can remember all of the changes they make so, in the end, the code I write tends to be more completely tested than the code that was tested in bulk.  The only difference between this method of development and “Debbie-Done” is the degree of completeness with which each developer is able “test” their code.

Avoid “Debbie-Done” with Test First Development
-----------------------------------------------

It is no wonder that many programmers I talk to think that test driven development takes too much time.  Compared to how they are programming, it does.

But think about this.  If you were to code like I do, suddenly writing test for every change you make suddenly makes sense because instead of you running the application and getting to the place that will trigger your code and observing if it will work or not, you can write a test for just the piece of code you are working on and run that each time you want to verify if it is working or not.  Yes, initially, this will take more time.  But over the life of the program, and I would say even over the time span that it takes to initially write the code, writing at least the Unit Test as you are writing the features will actually save you time.  Not only that, you will end up with test you can repeat every time the code is changed.

Even my method of “code, build, test, code, build, test” is “Debbie-Done” compared to writing test for each change as you make the change.
