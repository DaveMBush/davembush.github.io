---
title: Reasons Software Architecture Matters
tags:
  - best practices
  - software architecture
url: 4204.html
id: 4204
categories:
  - Software Architecture
date: 2017-02-14 07:30:00
---

Several weeks ago, I was talking to a programmer and we got into a discussion about the importance of software architecture. I maintained that having a defined architecture is important regardless of the team size, the person I was talking to asserted that architecture wasn’t necessary when there was just one person involved.

But here’s the thing. All software has an architecture. Even the most junior of programmers has an idea of how code should fit together. At issue isn’t really about architecture. It is about having a defined architecture, based on experience and best practices, that will allow the team to develop the software in question as efficiently as possible. Software architecture, at its core, says, “this is how we build software.”

To find the reasons why software architecture matters, it is helpful to think about what happens when there isn’t any defined architecture in place.  For the purposes of this article, I’m going to generalize on how architecture impacts teams and where appropriate show why that is also important when your team is just you. <figure>![](/uploads/2017/02/image-1.png "Reasons Software Architecture Matters") Photo via [VisualHunt](//visualhunt.com/)</figure>

<!-- more -->

Assumptions
-----------

In a team, having an architecture is important simply because it means we can make certain assumptions about how someone wrote the code you are now looking at.

Have you ever been in a situation where you wrote some code assuming that the programmers who would be using it had written their code in a particular way only to find out that they hadn’t and because of that, your code needs to be rewritten?

Or, how about the time you went to work on a bug? Once again, you made some assumptions that weren’t true so it took you much longer to fix the bug than it would have had you known that your assumptions were wrong.

Imagine what it would be like in those situations if the assumptions you were making were legitimate because everyone was using the same playbook. How much easier would that make your life?

Ah, but your team is just you, and you know how you put the code together. 

OK. Fine. 

But what about two years from now? Without a defined architecture, would you say you code things the same way every time? I know, even with an architecture, sometimes I “cheat” and my code doesn’t always follow the rules I’ve set out. If I do that WITH an architecture, I can just imagine how sloppy my code would be without it.

Arguments
---------

Get any two programmers in a room and you will almost always get two different opinions about how software should be written. A good architecture reduces the number of arguments so we can get on with the craft of writing code. I’ve been in situations where, even with an architecture, there are programmers who disagree. Some with good reason. But if you don’t have any definition, all you end up with is two opinions with no rule book to say who is “right.”

Cognitive Load
--------------

With so much to consider in software development, the fewer decisions we have to make, the better. This leaves brain power for solving problems that still need answers.

There are people who wear the same thing every day, or nearly the same, so they don’t have to make that decision. I’m actually one of those people. I kind of fell into this mode of dressing because I’m color blind and this is one of my ways of compensating. But I can tell you, it leaves me free to think about other things as I get ready in the morning.

This is another place where software architecture will help you regardless of team size. Even if you are the only one on the team, this is one less thing to think about.

Scrambled Eggs
--------------

Software built without an architecture will eventually take on the feel of scrambled eggs. Imagine how hard it will be to modify the code when you have no idea where the different parts of your code should go.

And once again, we end up with a system that is difficult to maintain simply because no one knows for sure where the various parts of the system should live.

Good Architecture
-----------------

You’ll notice I’ve been saying “Good” architecture. This is because I’ve been in situations where the architecture has not been defined tightly enough leaving too many loop holes. I’ve also seen architectures misapplied. It is important when an architecture is defined that the person defining is knows something about the tools that will be used and the environment they will be used in.

It is also helpful if the architectures that are defined, are defined by consensus rather than having one person defining it. We all have holes in our thinking. Someone may be a great architect, but maybe they suffer from the old saying, “when all you have is a hammer, everything looks like a nail.” More people bring more points of view. This can fill in gaps and can produce an architecture that can be used rather than one that will be resisted.

Success
-------

This is all about building successful systems. But just because you got some code working and into production doesn’t mean the code is going to hold up over time.

There is one guy I know who brags about how fast he can write code. Well, yes, you did get that into QA in a month. But, it took you three months to get it out of QA. The test of a good architecture is:

1. Does it help make the project successful?
2. Is it relatively easy to understand?
3. Has it been adopted by the team using it?
4. Is it generally accepted as a valid architecture in the community at large for similar software.
5. Does it make the code easier to maintain?

Getting Started
---------------

So, now I’ve convinced you that you should be using an architecture. How do you get started? It is a lot easier than you think. Most frameworks either have an architecture already defined, or somewhere the community has already defined one for it. Start there. Maybe refine it a bit and settle on something that works.

Conclusion
----------

If you don’t use an architecture yet, you are probably still thinking, “Yeah, but I don’t have any issues that this would solve.” To which I would answer, “How do you know? What if your wrong?”

You see, most of my reasons have to do with making software development easier or better. I’m not saying you can’t get something done without architecture. What I am saying is that with architecture you can do everything you are already doing better, faster, cheaper, and easier. Until you try it, you’ll never know if I’m right or not.
