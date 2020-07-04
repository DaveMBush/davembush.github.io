---
title: Magic Strings and Magic Numbers
tags:
  - 'c#'
  - magic numbers
  - magic strings
  - vb.net
url: 2783.html
id: 2783
categories:
  - Programming Best Practice
date: 2014-12-18 07:00:00
---

This past week a very old (last time I did work for him was in 2007) client of mine contacted me because their program suddenly started exhibiting a problem.  It seems that if a user enters a date anytime in 2015, the program displays an error message indicating that they need to enter a date greater than today and less than two years from today.

When I went to replicate the error in my debugger, I discovered this bit of code:

``` c++
if (Year < 100)
{
    if (Year < 15)
    {
        Year += 2000;
    }
    else
    {
        Year += 1900;
    }
}
```

<!-- more -->

So that, if you enter any date as 15, it assumes the date is 1915 instead of 2015.

Now, we are talking about code here that has been around since the DOS days, and I’m pretty sure this particular routine has been around since then too.  I’m sure when it was coded, the year 2015 seemed so distant, that no one considered that the program might actually still be running in some form.

But, that’s just the problem with Magic Strings and Magic Numbers.  They can appear to work for years, even decades, and then one day they show themselves for the evil they are.

So, how can you guard yourself against using Magic Strings or Magic Numbers? That’s just the issue.  You can’t.  Sometimes they are only obvious once they manifest themselves as a bug.  However, here are a set of common places you should look.

Are you working with dates?
---------------------------

Yeah, I know, this one is obvious since that’s what we are talking about.  But here are some other places you might want to look.

As I’ve considered how I might fix this bug, I’ve decided that what I’ll do is get the current year, find the two digit version and add three to it.  I’ll use that computed date as the century roll over.

But code is immortal.  That’s not the only place in this code that needs fixing.  The other place is the 1900 and 2000.  Those should be computed as well.  Not that I plan on being around, in the year 2100 and following, but as long as this code has been around, I should plan on the code being around then.  And it won’t be that much more work to compute those centuries as  century of today and century of today minus 100.

The only constant I can see using in the code above is the 100.  Even if it does change, I don’t think anyone will complain too loudly when it does.

Databases
---------

Another obvious place to look is in your database code.

Actually, if you aren’t careful, there are a lot of mistakes you can make here.  But for now, let’s just stick to magic string issues.

### Connection Strings

By now, this should be obvious.  Don’t include the connection string to your database in the code of your program.  At the very least, put it in your web.config or app.config file so it can be changed without recompiling your code.

### SQL

If you’ve done any amount of programming, you should know by now that your SQL belongs in your SQL database as a stored procedure.  That’s the best place for it.  However, I’ve had occasions where getting the SQL changed by the SQL gods, otherwise known as the DBAs, is so painful, that including the SQL in your code is much more practical.

Here’s a tip.  Keep your SQL code separate from your other code.  Put it in a resource file.  Put it in a text file.  Just don’t put it in your C# or VB.NET code.

### POCOs

Are you generating POCOs to accompany your SQL?  Assuming you can’t use Entity Framework and you have to generate your own POCOs, you should generate your POCOs from your SQL.  That way, if your SQL changes, your POCO code changes automatically.

### Schemas Change

And while we are talking about Schemas, I’ll remind you that Schemas change.  Do you have a strategy in place so that you can tell what version of the database schema you are using at any particular time?

The File System
---------------

File system issues have frustrated me on several occasions.  The most obvious one is the difference between Mac/Linux and Windows.  Now that .NET is going to be available on Mac and Linux, this is something you’ll want to pay attention too.  But, you should also pay attention to assumptions like drive letters.  Will you always want your code saved in this location.

We had an issue years ago with the same code base I referred to above when Vista came out and you could no longer save your data files with your program files.  It took several hours of refactoring to get that code into a location that the user had rights to.  That should be a lesson for you.  Once again, just because a hard coded value works today, doesn’t mean it will work tomorrow.

Dave’s Law of N
---------------

This next one is not one you’d naturally think of as a Magic Number, but it is.

You go to the customer and get requirements that say there will be two of something.  Maybe the contact record will have two phone numbers.  Or maybe they say there will be three of something.  It doesn’t really matter.  The magic number here is in the number of items that you need to store.

I’ve seen this violated in a number of ways.  But the interview question I now ask is something along the lines of, “You need to store a person class in the database with FirstName, LastName, BirthDate, HomePhone, and CellPhone.  Create a database schema to hold this information.

If they don’t come back to me with at least two tables, one for the Person and one for Phone numbers, I won’t hire them.  They get bonus points if they also create a third table so that we can classify the phone numbers in the phone number table.

And that’s where “Dave’s Law of N” comes in.

> If there is two of something there WILL be three of something. 

Code it today so that tomorrow we can add the third item without coding.  It won’t take you any longer to do it right today and it will save us time when they eventually realize they should have asked for more.

Well, I’m sure that’s not all of the places you should look.  But that’s a start to get you thinking in the right direction.
