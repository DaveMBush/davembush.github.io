---
title: Every Programming Issue Solved
date: 2021-09-26 08:06:30
tags:
---

As I've considered all the issues I've seen over the years and, as I've talked with and interviewed with other developers, I've come to the conclusion that our industry has two main problems.  Fix these and all the other issues we face will solve themselves.

<!-- more -->

## Not Understanding the Problem

The first sounds so simple and yet it is a lot more pervasive than one would expect.

You might think I'm talking about not understanding the problem we are trying to code for.  And of course, that is one obvious area this displays itself.

But, let's demonstrate other ways in which this manifest itself.

### Tools

I find that, for whatever reason, many programmers jump into a new development tool without fully understanding how the new tool works or the best practices for using it.

And, by the time most programmers have figured out what the best way to use the tool is, they've moved on to another tool. As an industry, we tend to use every new tool like we used the last tool we used because it is the only way we know.

The result is that we end up writing front-end code that looks more like backend code.  Or we write code using language A that looks like code using language B.

The above scenario has played itself out multiple times over my career. As the industry was moving from C to C++, most of the programmers I worked with would write C code in C++. Unfortunately, because you could do this and still end up with working code, most of them got away with it.

More recently, I've seen it with developers who knew C# backed programming and moved to TypeScript and front end programming. The result is "Redux" code that looks like Entity Framework code.  They are similar, but not the same.

### Design Patterns

Next, we tend to misuse design patters for the same reason.

For example, I once saw a project that was trying to use the "Repository" (Entity Framework) design pattern. The problem is, the "Repositories" look a lot like the "Business Logic" layer which means that a lot of the code that could be making one call to the database is making multiple calls to the database simply because the repository code isn't properly returning an IQueryable as it should be.

### Process

Another place I've seen this issue is with Agile and Scrum.

I can't tell you how many times I've heard people say that Agile/Scrum doesn't work. As I dig deeper, I find that they've never actually used the Agile/Scrum process. They've used something they were told was Agile/Scrum.

In fact, personally, I've yet to work for an organization that actually used Agile/Scrum. They all SAY they do. But each of them are either using Agile/Scrum as a reason for no process at all, or they are using it as a way to pick and choose the ceremonies they like without really being Agile at all.

### Best Practices

Similarly, I've seen this issue is in a recent Medium article saying the Single Responsibility Principle (SRP) is "garbage."

The problem seems to be, the person who wrote the article doesn't fully understand SRP and so, well, it MUST be garbage.  The clear indication of this is when he states that he tried to keep his classes from knowing about each other when he "suddenly realized" it would be so much easier if he didn't worry about SRP.

Um, yeah. No one said implementing SRP was easy.

And just in case you would dismiss this as "you don't know the exact issue they were facing," you are right, I don't.  But whenever someone tries to push back against an industry standard that I've actually seen benefit in following, time has informed me that the problem is almost always the way it is being implemented.  Just like with "Agile" and "Scrum."

## Granularity

This SRP issue leads me to my second observation.  Rather than being too granular, I've found that the problem is that the problem is we tend to not break a problem down far enough to be useful.

### The Scrum/Kanban Board

Going back to the Agile/Scrum example...

I would maintain that if you have task on your board that are considered "Work in Progress"... that is, items you expect to work on in the next sprint, and the number you've assigned does not reasonably represent work that can be done within 8 hours of effort, you probably have not broken the tasks down far enough.

If you are one of those rare groups who can estimate your tasks in larger increments and you are able to achieve everything you've put in your sprint, then you are probably doing the right thing and you can ignore my advice. My guess is you've broken the tasks down further within the tasks and that's why it works for you. Most places I've worked think a sentence estimated as a week is a task that can be reasonably estimated.

### Bugs

No where is it more obvious that we are not granular enough than that our code is buggy.

To be fair, bugs are part of writing software. But with few exceptions, most bugs could be avoided by breaking the code down into smaller pieces.

One of my best examples of this is from code I was asked to rewrite over 30 years ago.

The code in question had a continuous series of bugs that kept creeping up. My recommendation was to essentially put the code apart and put it back together again. Not something I would recommend today and the process took much longer than I had expected. However, my re-write broke the code down into functions that were essentially two to three lines of code per function all the way down the stack. When I was done, the code worked and we never saw an issue with it again.

### Deploying Software

To a more recent example, I had a discussion with an organization about deployment issues they are facing.  Specifically about how to release software to production when all the dependant parts are not ready.  The obvious solution to this is feature flags.  But, the problem with using feature flags is that they only work if your code granular enough that you can feature flag out the parts that are not ready.  This can be either because your code is not granular enough or, in the case of microservices, because your services are not granular enough.  Or, both.

### Understanding existing code

The last place the granularity issue manifests itself is in the existing code. Rarely have I worked on a system where the documentation is up to date and I had the luxury of relying on it.  And so, we are called on to deduce what the code should be doing by looking at the code itself.

But, if a function or method goes on forever, or has conditions that are a combination of conditions that are not obvious, then it is not granular enough.

My recommendation is to apply linting rules that apply complexity, line length and number of lines per method/function filters to your code to limit how much information is embedded in any one method or function. I know from experience it has forced me to think about the granularity of my own code as I bump up against the limits.

## Conclusion

When I perform code reviews, most of what I look at revolve around proper use of the tools we use and the complexity of the end result because if you can control these, just about every other issue will take care of itself.
