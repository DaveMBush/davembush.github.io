---
title: JavaScript Crazy Talk - Are you guilty?
tags:
  - business rules
  - javascript
url: 3265.html
id: 3265
categories:
  - Javascript
date: 2015-10-29 07:30:00
---

I heard this so frequently, I decided it is time to write about it.

> (When writing web applications)  Business rules always belong on the server.

One of the last conversations I had at the last place I was working was on this same issue.  And, I had a similar reaction a couple of years ago when I was doing a Selenium testing presentation and mentioned that the organization I was currently working for put all of the code on the client side and that the only thing the server did was save the data. Maybe you believe the same thing? Nothing is ever that cut and dry. ![image](/uploads/2015/10/image3.png "image")

First Rule of Rhetoric
----------------------

Now, the first rule of rhetoric, and the rule you must always keep in mind is this.  Whenever someone states an absolute, it probably isn’t true.  Including this one ![Smile](/uploads/2015/10/wlEmoticon-smile.png). Here’s the deal. At one point there were good reasons for not putting business rules on the client side.  Chief among them was that JavaScript was slow.  If if you accounted for the round trip time, performing business logic on the server was generally slower. Another reason it was nearly never a good idea to put the business rules on the client is that no one really knew how to write good, structured, testable JavaScript.  But JavaScript has grown up and this is not the problem it once was. And finally, and the only reason we have left, is that despite many attempts to obfuscate JavaScript, at the end of the day, JavaScript is available for anyone who wants to, to read and steal trade secrets from.  Things have gotten better, but the truth remains, if I really want to, I can figure out what your JavaScript code is doing.

What Is a Business Rule?
------------------------

OK, so I’ve shown that most of the old rules don’t exist.  We’ll come back to the third old rule later.  What I want to discuss is the chief, “it depends” issue. What, exactly, qualifies as a business rule? I have to admit, this question has bothered me for years.

### Three Tiered

Take a look at a typical description of a three tiered architecture.

> View <–> Business Rules <–> Data Access

I don’t know about you, but most of the applications I work on, the business logic layer does little more than pass the data from the Data Access Layer up to the View and back again.  There MAY be some transformations along the way.  It might combine data in a particular way.  But, really, the Business Logic Layer, strictly speaking, isn’t doing anything I would be uncomfortable letting my competitors know about.  There are only a few instances where I could argue that putting that code on the server was going to speed things up in any significant way.

### Model View Whatever

Then there is the Model View Whatever patterns.  You know, MVC, MVVM, etc.  In this pattern, our business rules either get stuck in the controller or we put them in classes external to this pattern.  Often this code does little more than decide what elements should be enabled or disabled, assist in validation, and auto populate fields that the user could populate themselves but we do it to make life easier for them. Once again, I have to ask, what about a business rule needs to be hidden or would work faster if we put it on the server?

Who Is Using Your Application?
------------------------------

Most web applications today are built by organizations to be used by themselves in Intranet environments.  In the last organization I worked for, there was no danger that someone would steal our code.  Most of them had no idea how to do it, and if they did, they wouldn’t be able to read it anyhow. By asking the question, “Who is using this application?” You are really asking, “What’s the risk of putting code on the client side?”  Most of the time for most of our code in most of the organizations, including even publicly facing web sites, there is very little risk involved in putting all of our code on the client side.

Is There Any Advantage?
-----------------------

Which comes down to the final question that needs to be asked.  Is there any advantage to putting code on the server instead of the client, or the other way around? In most cases, I think you will find, that the advantage of putting code on the client side far outweighs the advantages of putting that same code on the server side.  In some extreme cases, you may find that there is code that absolutely needs to live on the server.

Maybe I’m Missing Something?
----------------------------

OK.  So, maybe I’m missing something.  Do you agree or not?  Why?  Leave a comment.
