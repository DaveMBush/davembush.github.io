---
title: 10 Reasons Projects Succeed
tags:
  - project management
url: 4007.html
id: 4007
categories:
  - Scrum
date: 2016-08-09 18:30:00
---

We’ll get to Reasons Projects Succeed soon, but I need to do some setup work first.

I’ve been thinking about starting an Open Source project for a while.  The only issue was; I didn’t have an idea for a project that didn’t already exist.  Now I do.  So, I’ve begun the process.

The issue with starting a project like this is that I would much rather just start coding.  In fact, I would much rather not even make this Open Source.  But making it Open Source has forced me to face project management issues head on.

I’ve been listening to enough podcast recently to know that putting something up on GitHub isn’t going to make a project Open Source any more than it will make it successful.  Therefor, I’ve decided to start the project as though it had a team of people already working on it.  It is a team of one for now.  But, one thing I’ve learned in life is that having the structure in place to handle a larger team now will not just benefit me in the future, but it will actually help my small little team of me today.

I’ve started looking at other successful Open Source projects to see what they are doing and to determine what components of what they are doing I want to include in my project.  As I’ve gone through this exercise, the thought occurred to me, “If the organizations I’ve worked for implemented half of what these projects implement, the projects would have been run so much more efficiently and the projects that were in trouble may have avoided the trouble.” <figure>![](/uploads/2016/07/image-5.png "10 Reasons Projects Succeed")<figcaption>Photo credit: [Nguyen Vu Hung (vuhung)](//www.flickr.com/photos/vuhung/8576985602/) via [Visual Hunt](//visualhunt.com) / [CC BY](//creativecommons.org/licenses/by/2.0/)</figcaption></figure>

<!-- more -->

Clear Mission Statement
-----------------------

One of the most fundamental items that must exist in any project is a clear sense of what it is you are doing.  If you can’t explain what it is you are building in a couple of sentences, then how will you know when you are done?  How will you know you’ve succeeded? Once you’ve explained the summary, you might also want to go into some detail.  What makes this project different?  What problem does it solve?  What’s the short term vision?  Where might this project end up long term?  If there are other projects like this one, how is this one different? Why would anyone want to contribute to this tool?

Clear Expectations
------------------

The thing that strikes me most about well-run projects is that they set clear expectations for people who will contribute to the project.  Here’s just a few items that are typically covered:

*   Where to go if you have questions.  This typically includes email addresses, web sites, and other communication channels.  In a corporate setting you might also include phone numbers.
*   How to file a bug request.  Typically, you find this includes a line that says something about “steps to reproduce consistently”
*   How to ask for and/or add a new feature.  You’ll probably want to include something about making sure the feature gets accepted prior to working on it.
*   What constitutes “done” for a pull request.  Tests, documentation, code quality, etc.
*   Specific coding rules that need to be followed.  Even better if your style guide is automated in your build process and violations cause the build to fail.

Easy Development Environment
----------------------------

This one drives me crazy.  In just about every organization I go to, setting up my development environment takes a day.  Some places it takes three days.  Simply because I don’t have rights, or the items I need are scatter here and there.

Why not write the documentation for this, or better yet, take the time to write a script, that walks you through exactly how to get the development environment setup?  Life would be so much easier.

And for places that already have this, when is the last time you verified that the instructions or script still worked?  Stuff changes and we barely notice until the new guy shows up.

Continuous Integration Server
-----------------------------

It sounds crazy in this world, but how many places still don’t have a continuous integration server.  And yet the most successful OS projects do.  Correlation?

Unit Tests
----------

Testing.  We all hate it.  And here again, the best projects make sure there are test.  I’ve covered testing in multiple places before.  We’ll just leave this at, “You need tests!”

Application Level Tests
-----------------------

And if you are writing an application, you need application level tests.  At least a few to make sure everything works together.

Centralized Communication
-------------------------

It amazes me how many places still use email as the primary way of communicating.  What’s wrong with that?  Well, people get included who shouldn’t.  Don’t get included that should.  And finding the email when you need it is impossible.  Which version of the email has what you need anyhow? Most projects fail at the communication level.  There are tools for that.

Project Management Tools
------------------------

Find a project management tool and use it.  Actually, find a project management tool that your team will use and use it.  There is a lot of crap out there.  Don’t let the sales literature pick your tool.  Try the tool.  I’ve finally been introduced to a VERY popular tool where I’m working now.  I can’t believe it is so popular because I’ve used tools that are SO much better.  Better yet, find a useable tool that doesn’t just integrate with other tools, but merges with other tools.  You shouldn’t have to leave GitHub to use your Kanban board, for example.

Swarms vs Silos
---------------

Here’s another places that drives me crazy.  Did you ever notice that in an Open Source project, the QA, Documentation and Coding all happen off of GitHub.  But in most organizations, those are all Silos?  What would happen if your QA, Documentation, and Coding people all worked out of the same space in your office.  In our situation, I have no idea if we even have documentation people.  We certainly don’t have QA people working WITH us daily.  And the programmers are even spread all over the place.  And we wonder why nothing gets done and we have communication problems.  We might as well work from home.  At least then there wouldn’t be an illusion that we were working together and we’d rely on collaboration tools more.

Opt-In
------

OK.  This one is going to scare the managers.  But what would happen if the team of programmers where you worked were able to choose what projects they worked on?  I know in some places this wouldn’t work because there is only one project.  But in larger organizations? In most places, you get hired for a gig and then you get assigned to some project.  In some cases, you get hired for one project and assigned to another.   So even the choice you thought you had, you don’t have.

Here are some advantages to this:

1.  Poorly run projects would get abandoned.
2.  Projects with no viable reason would get abandoned.
3.  Programmers would work on things they enjoyed.
4.  Product owners would have to contribute.
