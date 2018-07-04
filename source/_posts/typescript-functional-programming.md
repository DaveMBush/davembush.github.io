---
title: Enforce TypeScript Functional Programming
tags:
  - functional
  - tslint
  - typescript
url: 4399.html
id: 4399
categories:
  - none
  - TypeScript
date: 2017-07-25 06:30:00
---

As consistent readers of this blog are aware, I’ve fallen in love with Functional Programming.  But I also live in a primarily Angular/TypeScript world where some code is still Object Oriented and other code is more Functional in nature.  And while TypeScript lets you do some Functional things, I’ve found it hard to force functional concepts in TypeScript.  So, I’ve gone searching.  Wouldn’t it be great if there were some sort of flag you could set that said, “Hey compiler, I’m in ‘Functional’ mode now!”  And the compiler would make sure that you never used a conditional statement, never accessed hidden parameters, and never mutated state? Well, I think I’ve figured out some of how to manage all of this using nothing more than TypeScript and some TSLint rules. <figure>![](/uploads/2017/07/2017-07-25.jpg)<figcaption>Photo credit: [archer10 (Dennis) 99M Views](//www.flickr.com/photos/archer10/15127010075/) via [VisualHunt.com](//visualhunt.com/re/52cd50) / [ CC BY-SA](//creativecommons.org/licenses/by-sa/2.0/)</figcaption></figure>

<!-- more --> 

Immutability
------------

Several weeks ago, I demonstrated how to [protect against immutability using freeze](/really-use-ngrx-better/).  But, wouldn’t it be better to catch immutability issues at compile time? There are a couple features in TypeScript that do just that. First, we can use the `readonly` keyword on a field to ensure that it doesn’t change.  And second, we can declare arrays using `ReadonlyArray`.  For this to be most useful, we are going to want to provide an interface for all of our data that needs to be immutable.  This is how I typically use NgRX.

Enforcing Functional
--------------------

Now, that is as far as it goes for support in TypeScript itself, as far as I am aware.  But, we can get more support by using TSLint.  If you are using the Angular-CLI, this gets installed when you create your project.  If you aren't using the Angular-CLI, or you are using some other framework, you are on your own for getting that installed and setup. There are two problems we need to overcome in order to use TSLint to solve our "switch into Functional mode" issue.  First, is that we need a set of rules we can use.  The second is that we will need some way of isolating where those rules take effect. Fortunately, we can create a separate `tslint.json` file for each directory.  And since all of my Functional code is isolated to my NgRX state management stuff, I can put a file in that directory and it will take care of my Functional needs while leaving my normal `tslint.json` file for the rest of my code. The other thing you will want to do is that you'll want to install [tslint-immutable.](//www.npmjs.com/package/tslint-immutable) tslint-immutable adds some rules specifically for immutability that are not included in the core implementation.  I love plugable systems. I'll let you read the documentation on the site for the rules that it adds.  Who knows when you'll be reading this and the package may have changed by the time you get here.

pre-commit hooks
----------------

Having a good set of linting rules if of no use if they get ignored.  Right? So, another package I install into my projects is pre-commit.  Be warned though.  If you are using Windows as your development computer, you'll also need to install [coreutils](//gnuwin32.sourceforge.net/packages/coreutils.htm) because pre-commit assumes you are able to run bash. But of course, someone could comment out or remove the pre-commit hooks or otherwise force a commit.  This is why you also want a good code review process in place for your team. When I review code, I check several items regularly.

*   What changed and was it changed in a way consistent with how we want things coded.
*   If something wasn't coded correctly, is there a linting rule we can put in place to correct it?
*   When I pull down the code, can I compile for production as well as for development mode?

Point 2 is critical.  The more reviewing the computer can do for you the better off you'll be.

Conditionals
------------

What I haven't been able to find is a rule that will prevent conditions from showing up inside of observable functions or their array cousins.  If you know of something, please let me know in the comments below.

Code
----

Now, a little code would be nice to flesh this all out.  So, this is what I'm currently using: First, the scripts and pre-commit sections from my package.json file: And then the tslint that I put with my NgRX code: If you have any other tips for how to force TypeScript into being more Functional, I'd love to hear them.
