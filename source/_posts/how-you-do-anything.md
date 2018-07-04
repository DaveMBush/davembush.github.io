---
title: How you do Anything ...
tags:
  - agile
  - best practices
  - extreme programming
url: 4573.html
id: 4573
comments: false
categories:
  - Programming Best Practice
date: 2018-02-20 06:30:35
---

I've been programming now for 30 years. Over those thirty years, and more so over the last five to ten years, I've become increasingly frustrated by the attitude of managers and programmers alike toward programming. One programmer I know is pretty vocal about this attitude.  All he seems to care about is how fast he can write the code.  "I got that application done in a month!"  And then he'll complain about how it is everyone else's fault that he spent the next four months fixing bugs. And it is no wonder he has this attitude.  Most of the managers I've worked for will acknowledge that there is a lot of technical debt, but the pressure of getting the code written always outweighs the pressure of the debt. I've said for years that I'm not at all surprised that software has bugs.  What surprises me most is that any of our code works at all.  If we are honest about our code, we recognize that our code is worse than a store front on the wild west.  A nice facade on the front that everyone sees (if we are lucky) but look behind the facade and the store is barely standing up because the design, architecture and lumber is so bad. But, if it is true that "How you do anything is how you do everything."  shouldn't we spend a little time practicing quality in the not so obvious places so that we can get in the habit of quality?  Maybe a culture of quality will rub off into our code and produce code that really does get written quickly and has very little technical debt.  Not just in the area of bugs, but in flexibility, architecture, and design. So, what could we change in our programming practice that wouldn't require persuading a manager to make a change in how the whole organization worked? <figure>![](/uploads/2018/02/2018-02-20.jpg "How you do anything...") Photo on [Visualhunt](//visualhunt.com/re/cecba2)</figure>

<!-- more --> 

How do you Dress?
-----------------

When I first started programming, I had to wear a suit to work.  I forgot my tie one day and actually got reprimanded.  This, despite the fact that the programmers were tucked away in a back room that you had to know existed to get to.  That is, there was no way one of the customers our business worked with was going to find us.  I hated it. The pendulum has in the opposite direction.  I've been on several interviews where I've explicitly been told to NOT wear a suit. But I do wonder.  Does how we dress have an impact on our code?  If we don't care much about how WE look, might we not care that much about what our code looks like?  Does it matter?  I think it might. Might it also subconsciously impact other's view of the quality of code you produce?  You know the adage.  Always dress just below your manager.  But what if your manager dresses like a slob?  Currently, my signature work clothes are black jeans, button down shirt and a sweater.  But, I'm thinking of bumping it up a notch.

What does your desk look like?
------------------------------

There was a guy I worked with early on in my career who always left his desk in a condition that looked like no one had worked at the desk in months.  Everything was put away.  Several years ago, I decided to take on that habit. Funny story.  A couple of gigs ago, a manager from three levels up came down to visit after I had left for the day.  She looked at my desk and said, "Does anyone work here?"  My manager said, "YES!!! Don't touch anything!"  Guess she was hunting for stray equipment. I would say, this simple act was the pivot point for me when I started to care more about my code beyond "Does it work?" There is something about working in a clean area slightly dressed up.  Other parts of your life follow along for the ride.

Linters
-------

One of those areas is what your code looks like.  A linter can be configured to enforce style rules on your code.  And can also catch dumb mistakes.  The more I use linters, the more convinced I am everyone should use one. Here is some of the ways you will benefit, even if you are the only one using a linter.

*   Your code will always follow the same format.  This will make the code easier to read and understand.
*   As I mentioned, some rules help catch dumb mistakes before you even run the code.
*   Just like a clean desk feels inviting, well formatted code feels inviting.  The converse, poorly formatted code, adds to the stress level of working on the code.

Learn Your Tools
----------------

I am amazed at how many places I've worked where they expect you to learn a particular tool on the fly.  No training.  "We are now using X on this next project.  Download it and start using it!"  And then they wonder why the project is such a disaster.  Bugs everywhere. Want to look like a rock star?  Want to be the guy the boss ask "what should we do?"  Be the guy who knows that new tool better than anyone else. In the process, you'll also be the guy who uses the tool the way the people who developed that tool intended it to be used. Here's a truth I've observed.  Everyone wants to use whatever tool they've just started using like the last tool they used.  And then, when it doesn't, the new tool "doesn't work."  This one truth has done more to hold our industry back than any other one thing I can point to.

Design Patterns
---------------

Learn the basic design patterns for your environment.  But go beyond learning the pattern.  Learn why the pattern exist.  What problem does it solve?  Be able to recognize counterfeits.

Architecture
------------

Along with Design Patterns, learn the basics of Architecture.  Here's a hint.  All good architecture answers the question, "How can I write code that is not dependent on any other code?"  This is one of the reasons I love Functional Programming.  By design, Functional code is not dependent on other code.  Dependencies are passed into the function.  But you can achieve similar objectives using Object Oriented programming languages or even Procedural languages.

Single Responsibility
---------------------

Granular is better than Monolithic!  Don't be afraid to split things up.  Use components.  Compose classes of other classes.  Components of other components.  And most important of all, don't create a Utils class of any kind.  In some languages this is easier than others.  But if you can, any method you have in a Utils class should be a function in its own file.  If you can't do that, maybe each function belongs in its own class unless there is a strong argument for putting them together in a class that can be reasonably named in a way that obviously groups those methods. Another pet peeve of mine is putting multiple classes or interfaces in one file. Split them up!  Don't make me go hunting for the source of a class. The ONLY time you might be excused from splitting things up is if the only place the code is being used is in the file's main class.  But once you make that class available to the outside world, out it comes.

Alternate Languages
-------------------

This should go without saying.  But the more languages you learn, the better programmer you will become.  I remember when I started learning C++, I had a hard time understanding references.  And then I started learning Clipper (a dBase III compiler) and something they said made references make sense suddenly. And this has been my experience all along.  Every new thing I learn helps me see problems in a slightly different perspective.  Making my code in every language I know that much better.

Code Reviews
------------

So far, everything I've mentioned is something you can do on your own.  This last one will require at least one other person.  Maybe you can buddy up with someone at work for this. It amazes me that in 30 years of programming, only three of those years have been in an environment where my code was ever reviewed by someone else.  But getting another set of eyes on your code is helpful if for no other reason than someone else has had to try to understand what you did while you still remember instead of waiting years and you've forgotten or moved on.  None of us are as smart as all of us.  Take advantage of another pair of eyes on your code, even if that's not a formal thing where you currently work.

What Else?
----------

What else you change the quality of our code by changing our own habits?  Leave a comment.
