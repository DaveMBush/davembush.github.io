---
title: '5 Reasons Learning Terminology Increases Your Effectiveness [As A Programmer]'
tags:
  - learning
  - programming
url: 3214.html
id: 3214
categories:
  - Programming Best Practice
date: 2015-08-27 07:30:00
---

![image](/uploads/2015/08/image3.png "image")

A couple of weeks ago I wrote a post “[7 C# Interview Questions to Weed out Losers](/7-c-interview-questions-that-weed-out-the-losers/)” which was my most popular post yet.  As of this writing, it has received over 13,000 views.  It also got a lot of comments.

While there are a lot of things I could respond to, the one I want to focus on today is what I would call, “The fallacy of concepts over terminology.” While none of the comments actually come out and say this, several imply that knowing the concept but not knowing the proper term for it is enough.  In conversations with people I’ve worked with, I’ve received similar feedback.  In fact, as recent as three years ago I actually told someone, “If you want someone who can pass some sort of test, I’m probably not your guy.  If you want someone who is an awesome programmer, I’m your guy.” But three more years of experience has changed my mind.

Knowing the Proper Terms Clarifies Communication
------------------------------------------------

<!-- more -->

> “'When **I** use a word,' Humpty Dumpty said, in rather a scornful tone, 'it means just what I choose it to mean — neither more nor less.'” – Through the Looking Glass, by Lewis Carroll

Here are just a few examples of how knowing what a term actually means helps communication: As I’ve written before, I’ve been using Sencha’s EXTjs at my day job.  When I first started using this environment, the first thing I was told was that it uses the “MVC Architecture.”  Now, to me, MVC has a very specific meaning and more importantly implies certain qualities about how the code should be written.  Primary among them is that the Model, View, and Controller are loosely coupled.  Anyone who has used EXTjs 4 knows this is not necessarily true.  What they call the “Controller” is more accurately called a “ViewController” but even then, it isn’t really.  You can’t have a controller without having a view, which makes the whole thing quite hard to test.  This whole mislabeling thing set me back about two weeks in my attempt to learn the framework.

Another example from my work place.  We’ve created our own ORM to access the DB2 database we have to work with.  Why?  Because DB2 doesn’t play nice with Entity Framework.  But here again, I was told that this code implemented a design pattern that it doesn’t really implement.  Since the number of times I have to interact with this code is trivial, it was not as costly to my learning.  But it did cause confusion.

Probably the most notable misuse of a word in our industry is the word “Agile.”  As I’ve been interviewing, I’ve run into a lot of companies who call themselves “Agile” but I’ve found that what they mean by “Agile” is very different from how the Agile Manifesto defines Agile.  In fact, one of the questions I ask during the interview to companies who have promoted themselves as “Agile” is, “How do you define Agile?”  One of the first places I asked this question replied, “Oh that just means we program iteratively.” So, knowing the proper term for a concept, makes your communication clear.  But that doesn’t explain why I expect someone I’m interviewing to know the term I’m using.

Knowing the Proper Term Shortens Communication
----------------------------------------------

Way back when I was first learning object oriented programming with C++, someone asked me what polymorphism was.  I couldn’t answer because I’d never heard of the word or hadn’t taken the time to find out what it means.  I can’t remember exactly.  But I had the same reaction many of you did.  “I know what virtual functions are and I know how to give a function multiple implementations by changing the signature, I just don’t know the word for it.” But you see, it is much easier to ask, “What is polymorphism?”  Than it is to ask, “What do the keywords ‘overridable’, and ‘overrides’ do?”  And then have to follow up with “what does the ‘new’ keyword do when placed in front of a method declaration?” Spoken languages have terms for the various parts of speech.  If you are a programmer, I’m sure you know about this, even if you can’t name them.  But, let’s say you were a professional writer or an editor.  You probably wouldn’t be able to even get a job if you couldn’t talk about the various parts of speech using the proper terms.  And yet, many of us expect that we should be allowed to write code without having the same ability.

Which leads to my next reason.

Knowing the Proper Term Aids in Learning
----------------------------------------

My family is a languages family.  My wife knows English, French, Latin (from teaching the kids), and Biblical Greek.  My daughter was an English major and learned Latin during High School.  My oldest son, knows English, Latin, Greek, and a bit of German.  My youngest son knows English, Latin, a bit of Greek and German.  I barely know English well enough to get these post written every week.  Which gives me a unique perspective.

What I’ve learned as I’ve watched my family play with languages is that because they know the parts of speech in the various languages, as they pick up a new language, they can talk about a word being in a particular tense, or case, or whatever and everyone knows exactly what they are saying, making it that much easier to pick up a new language.

In fact, while my son was learning German in High School, he would come home and talk to his mom and sister in German and they would be able to make out most of what he was saying because they had enough back ground in other languages and when they didn’t know something they could talk about it using the terms for the various parts of speech.

This was true for me as I moved from C++ to Java and then to C# and VB.NET.  Because I already knew what polymorphism was, all I needed to know was the mechanisms in the other languages that made that feature happen.

If You Know the Terminology, You Probably Know the Concept
----------------------------------------------------------

One of the complaints I got specifically accused me of asking questions that only proved that the applicant could pass a test.  And this is true.  If all I were to ask were these questions, this would be a bad interview indeed.  But here’s what I’ve found.  Most of the time, if you can answer the questions I’ve asked in that article, you are much more likely to be able to answer the questions I really care about.  “Are you a hack, or do you really know what you are doing?” The market is flooded with programmers with less than 5 years of experience being presented by recruiters who know nothing about programming.

Using the metric of charity over what I see coming in for interviews, here is what I think is happening.

Recruiter sees an applicant that has most of the buzzwords on his resume that the company they are recruiting for is looking for.  But they are missing a critical buzzword, so the recruiter adds this prior to passing the resume on.

Having been on a lot of interviews over the years, I have to assume that this works enough of the time to make it worthwhile because there are some places that I’ve interviewed where I haven’t had to prove I was technically capable at all.  In fact, I’ve been on a few interviews where the resume they have for me doesn’t look anything like what I gave the recruiter.

And so, those of us on the receiving end of this have to have some quick way of determining, “do you know anything?”

Knowing the Concept Isn’t Enough
--------------------------------

Keep in mind, I used to think that knowing the concept WAS enough.  But what I’ve come to realize is that if I know a concept but I don’t know the proper terms for it, I probably don’t know the concept as well as I could.  Sure, I can write code all day long.  But, it doesn’t mean I will write the best code that I can.  It just means that I’ll get the job done.

But once I spend the time to learn not just the concept, but the words that describe the concept, I’ve become a student of my craft.  I’ve proven that this is more than a job.  I’ve become a true professional.  Anything less and I’ve proven that I’m a really good hack at best.

I realize that this may offend some of you.  So, let me soften this.  I’m not saying you are a bad programmer if you don’t know the proper terms for what you are doing.  What I’m saying is you aren’t as good of a programmer as you could be.  And for those of you who might disagree, the question I have to ask is, “how do you know?”  If you are still defending not having to know this, I have to make the assumption that it is because you are happy not knowing the terms.  Meaning you have nothing to compare where you are to where I am proposing that you should be.  Why not try becoming a student of programming and seeing for yourself if what I am saying has any merit or not.