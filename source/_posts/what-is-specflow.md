---
title: What is SpecFlow…
tags:
  - mstest
  - nunit
  - specflow
  - tdd
  - testing
url: 2540.html
id: 2540
categories:
  - SpecFlow
date: 2014-06-12 13:30:00
---

…And why do I want it?
----------------------

![SpecFlow](/uploads/2014/06/SpecFlow.png "SpecFlow")That’s what I kept asking myself every time I saw this product. Well, the last time I looked, something caught my eye enough that I decided to download it and take a look. I'm really glad I did it. So, let me attempt to explain what [SpecFlow](//www.specflow.org/) is.  First, while you can get SpecFlow+ Runner to run your test, this isn’t a completely new testing platform. Instead, it is a testing platform that works with whatever testing platform you are using to test your .NET code. So, it works with NUnit, MSTest, xUnit and MbUnit. This is good for my situation because any test I write with SpecFlow will continue to work with the tools I already have in place. Including my current test runners ([ReSharper](//www.jetbrains.com/resharper/)) and my Continuous Integration system ([TeamCity](//www.jetbrains.com/teamcity/)). I wasn’t interested in adopting a platform that wouldn’t work with these two platforms.

Where I got Confused
--------------------

Now the part that kept me from looking at this framework for such a long time was the [home page](//www.specflow.org/). If you go there, you will see the “Three easy steps.” 1) Specify, 2) Automate, 3) enjoy. Well, at first glance it looked like was going to have to write my test in English and then write my test in code and then I could enjoy. Why would I want to do that? So, the part they forgot to mention is that once you’ve specified your test, the Visual Studio plugin will generate your test code for you. For NUnit it actually writes the NUnit test for you. The code you do have to write is the code that translates the individual steps. But even that code gets stubbed out for you. Once you have enough of those stubbed out, to write a test, you just write the test in English, save the file, and run the test. Now that I finally understand this, I like most of what SpecFlow does. But there are a few things I wish it did differently.

What I like:
------------

1) It puts my test in a Given, When, Then structure like I mentioned a few weeks ago. 2) The class files it generates are all partial classes. This allowed me to keep my inheritance structure that I’m dependent on for my current NUnit testing framework. And allowed me to work around one of the shortcomings I’ve found. 3) The methods are all virtual, again, allowing for extension and flexibility. 4) Intellisense is enabled so you can see similar sentences you’ve already used as you are creating new test. 5) You can create tables of parameters for test scenarios, similar to how you can pass multiple parameters to an NUnit test with the TestCase attribute or the TestCaseSource attribute. 6) To write the supporting CSharp, code I didn’t have to learn anything new. 7) I can give the English language, none code, document to the business user and they should be able to read the test well enough to tell me if the test is an accurate reflection of what they want the system to do.

What I don’t like:
------------------

1) I’ve become pretty dependent on the NUnit TestFixture attribute that allows me to pass parameters to the entire test. I use this to control the testing of multiple browsers. But, I’ve recently figured out a way to run all of my browsers simultaneously rather than sequentially so I won’t need this feature in the pretty near future. Once I’ve got that working, you can be sure I’ll share it here. So, don’t forget to subscribe to this blog if you haven’t already.
