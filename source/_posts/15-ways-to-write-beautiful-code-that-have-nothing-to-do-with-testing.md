---
title: '15 Ways To Write Beautiful Code [That Have Nothing To Do With Testing]'
tags:
  - code
  - programming
url: 3190.html
id: 3190
categories:
  - Programming Best Practice
date: 2015-08-06 07:30:00
---

![image](/uploads/2015/08/image.png "image")

I got a question this last week that I answered very briefly but I felt that to answer it completely would take a blog post.  So here’s the blog post.

> Should the author of a piece of code be responsible for more than just unit testing, or does peer review have a play?

<!-- more -->

Loaded question, right?  Obviously, the guy asking this question believes that an author should both unit test AND have peer review.  And I agree.  But to limit the role of a programmer to even those two things is a very narrow view of programming. Then I got to thinking about it and realized that most of what we’d be trying to accomplish with a code review encompasses everything we’d want to include in a list of things a developer is responsible for.  Further, a code review is mostly a review that ensures that 1) the code does what it is supposed to do, and 2) the code is easy to maintain in the future.

In other words, all that the word “Peer review” or “Code review” encompasses is largely about code clarity.

Code clarity addresses the issue we all have when we pick up a new piece of code.  Can you understand it easily?  I just listened to a [DotNetRocks episode](//dotnetrocks.com/default.aspx?showNum=1172) that was all about the fact that we spend most of our time reading code and yet no one is talking about how we might do this better.  One way we can read code better is by writing it more clearly in the first place.

Here are 15 ways you can make your code easier to read:

Make your code pretty
---------------------

I remember back when I was doing Clipper programming, we had one guy who would always show code that was ready to be printed in a text book.  I mean, regardless of what you had to say about what he had actually coded, the visual presentation of the code was a thing of beauty.  He had us believing that all of his code looked like that.  Then he went to work for some other company and we actually looked at the other code he had written.  It looked just like any other code.

But it taught me a lesson.  A lesson I, unfortunately still need to be reminded of.  Pretty code is easier to maintain and implies a lot about the quality of the code.  Just like you wouldn’t go to an interview without wearing a suit because how you dress implies something about how you’ll program, your code should imply the quality by what it looks like before you ever start reading it.

Establish and obey naming conventions for your code.
----------------------------------------------------

I’ve written about this before.  Most notably when .NET first came out in the article about [Hungarian Notation](/hungarian-notation-use-what-works-spit-out-the-bones/).  As I point out in that article, not using Hungarian notation doesn’t mean you don’t have any standards.  Sure, you don’t want to prefix all of your integers with a lower case “I”, because you don’t care what kind of number it is.  But it is valuable to prefix a button with “button” so that it is easier to find in your Intellisense, or if you need to search your code for what you named it, it is much easier to find all of the buttons in your code this way.

But there is another reason to establish and obey naming conventions.  Once you’ve done this, it makes it easier for everyone who is using those common conventions to read your code.

For example, where I work, we have what I consider a very odd naming convention.  All of our POCO classes (we generate this ourselves because we are using DB2) are all UPPERCASE\_UNDERSCORE\_SEPARATED.  And all our properties of POCO collections are named UPPERCASE\_UNDERSCORE\_SEPARATEDs.  It used to drive me crazy, mostly because I use [ReSharper](/resharper) religiously and the default rules aren’t setup to handle this odd convention.  But, it is a standard and, I can tell you now, you open a file that is using a POCO and you know you are dealing with a POCO.  No question about it! While I still don’t recommend the practice, I tell you that example because it illustrates how much a coding standard can instantly tell you something about the code you are working on before you ever start reading the code.

Establish and obey a common architecture.
-----------------------------------------

One of the biggest problems I have in reading other people’s code is if I can’t understand why they put things where they put them.  But having a common architecture that is easy to explain helps alleviate a lot of this problem.  One of the reasons I like working with frameworks is because much of the time these decisions are already made for you.  For example, just having a simple three tiered architecture that separates the View, Business Rules, and Data Access into different parts of your code can go a long way toward making code easy to read.  Within this basic framework you can layer in Model View Presenter, Model View Controller or any other design patterns.  Follow the mantra, “A place for everything and everything in its place” and your code will be much easier to follow.

Name objects as nouns, methods as verbs.
----------------------------------------

This is so basic that I almost feel like I should have to write this here.  But I still see the principle violate all of the time.  So, here it is.  Obviously, if you aren’t following an object oriented methodology.  Say you are doing Functional or Procedural programming, you’ll need to adapt this rule to your environment.  But for OOP guys and gals.  Stop breaking this rule.  It is a sign of immaturity.

Name variable what they are.
----------------------------

Similarly, stop with the short variable names unless you are using a language that is still so archaic as to require it.  I’ve written about this before.  Twice actually.  [With rants about using i, j and k as variable names](/tags/naming-conventions/).  The point here is that you want to be able to read your code and to do that your code has to tell you something about what you are doing.  Short variable names almost always never assist in code readability.

Don’t include a noun in your method.
------------------------------------

This is different from just saying that method should be verbs.  There are a lot of places where I’ve seen code that has a verb AND a noun as a method name.  If you have a method that has both a noun and a verb in it, your class is probably trying to do too much.

Wouldn’t it look funny if you had a Person class that had a method in it called FirstNameToLower()?

Establish a [cyclomatic complexity](//en.wikipedia.org/wiki/Cyclomatic_complexity) threshold for your methods and obey them religiously.
----------------------------------------------------------------------------------------------------------------------------------------

Cyclomatic complexity takes into account how many statements have to be executed and, most importantly, how many conditionals have to be executed in your code.  The easiest way to reduce cyclomatic complexity is to reduce the number of conditions.  But aside from that, you can make your code much more readable by eliminating nesting where ever possible.

One of my favorites is instead of writing code that looks something like this:

``` csharp
void SomeMethod()
{
   if(x != y)
   {
      // do stuff here
   }
}
```

You can make your code much more readable by writing it like this:

``` csharp
void SomeMethod()
{
   if(x == y) return;
   // do stuff here
}
```

Similarly, instead of nesting when this doesn’t work, as with while statements that contain other while statements, why not create a method for each while statement?

Make code “self-documenting”
----------------------------

That is, if you feel your code needs a comment, you probably have violated one or more of the other readability suggestions.  This is not to say that you should never add comments, but they should not be a replacement for readable code.  I generally leave comments for WHY I did something and leave the WHAT I did to the code.

Don’t add comments for things that you can deduce by reading the code.
----------------------------------------------------------------------

“This next line increments loopVar by 1” does no one any good and could eventually end up being litter in your code that makes the code harder to read.  Why?  Because it is possible for loopVar to get renamed, or removed and the comment could stick around forever.  Until we get compilers that verify our comments as well as our executable code, referencing variables in comments is a generally bad practice that should be avoided.

Is the code [SOLID](/pluralsightSOLID)?
---------------------------------------------------------

*   Single Responsibility of classes
*   Open/Close Principle
*   Liskov Substitution Principle
*   Interface Segregation Principle
*   Dependency Inversion

Is the code testable?
---------------------

If you follow all the rules above, the code should be easy to test.  But, this is something that you should review your code for explicitly.

Will the code fail?
-------------------

Can anyone write code that uses any particular method that would make the method crash, throw an exception, or otherwise do something that was never intended?

Code duplications.
------------------

I don’t know about you, but I’m pretty sure I’ve got code that looks practically the same littered throughout my code base.  One of my current goals is to make myself create a new method any time I’m getting ready to copy and paste more than three lines of code.

100% Code Coverage
------------------

Assuming you have unit test, do you have 100% code coverage on all of the methods that have a cyclomatic complexity score greater than 1?  I’m talking about code you’ve written.  I’ve written before about [what code should be tested](/100-code-coverage/) and what code doesn’t need to be tested.

Learn the vocabulary of your language
-------------------------------------

John Sonmez wrote a very compelling article in 2013 about [what makes code readable](//simpleprogrammer.com/2013/04/14/what-makes-code-readable-not-what-you-think/).  In it he says that just like when we learn to read.  Code that is readable by a senior level developer may not be readable by an entry level developer simply because the entry level developer doesn’t have a firm grasp of the language.  Like John, I’ve been criticized for using the ternary operator because it is too terse.  Not descriptive enough.  Un-readable.  And I’ve always responded that arguing that the syntax isn’t clear is like arguing that a sentence isn’t clear because it uses a word you aren’t familiar with.  The word may actually be the perfect word for what the author is trying to communicate.  Your understanding of that word does not make the writing bad.  It just means the reader has some more learning to do.
