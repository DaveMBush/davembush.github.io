---
title: 'NUnit, Unity Dependency Injection, MOQ and Private Fields'
tags:
  - dependency injection
  - nunit
  - reflection
  - unity
url: 2670.html
id: 2670
categories:
  - TDD
date: 2014-10-23 06:00:00
---

I had an interesting puzzle to solve this week that I thought I would share with you in case someone else is looking for a similar solution. There was some code that I needed to test that ultimately called into the database. Since this is a UNIT test and all I was interested in testing was one specific function and the state of one specific field in another  object, I had neither the need, nor the desire, to let that call to the database happen.  Since MOQ is  my mocking framework of choice, I wanted to mock out the database object the method was using so that it would return whatever was expected without actually calling down into the database. There were several problems.  The first was that the database object is a private field in the class I was testing and it got created by the constructor.  Second, the code that needed to use the database object is passing the object by reference (using the “ref” keyword) so that I could no setup my class that needed that object to just ignore it. There were some other items I needed to inject, but they were pretty straight forward.

<!-- more -->

Disclaimer
----------

But first, I realize that some purist out there is going to leave me a comment that says you shouldn’t use a Dependency Injection container in your unit test.  My answer to that is, yes, ideally, one should not do this.  But, not everything is ideal.  Not every project is a “green field” project (and in fact about 95% of them aren’t).  And in some cases there is so much technical debt involved that the best we can do is a hack.  Read, [Working Effectively with Legacy Code](//www.amazon.com/gp/product/0131177052/ref=as_li_tl?ie=UTF8&camp=1789&creative=390957&creativeASIN=0131177052&linkCode=as2&tag=davmbusnetapp-20&linkId=2ULNYA2XTSEGDPLA)![](//ir-na.amazon-adsystem.com/e/ir?t=davmbusnetapp-20&l=as2&o=1&a=0131177052) for other illustrations of “hacks” to get code under test. So, there’s the problem.  It may not make a lot of sense yet, especially if you are not familiar with the tools.  So let’s walk through some code.

The Code
--------

The main method I wanted to test was hanging off a POCO class.  It is using what we call a “visitor” pattern (though I’m pretty sure this is not what the pattern is supposed to look like).  So we have an Accept() method hanging off of it that knows how to store the poco into the database. That method has a method it calls that hangs off a crud object called Create() that takes the POCO and a reference to the database. I want to mock the Create() method so that it never actually gets called.  The method should return the POCO as part of a list of the POCO’s type. Mocking out the crud object is pretty straight forward:

``` csharp
_mockCrud = new Mock<ICrud>()
```

and then registering it with the Unity DI container:

``` csharp
container.RegisterInstance(_mockCrud);
```

The real issue, and the main point of this post, is that when I wanted to call Create() what I needed to do was:

``` csharp
_mockCrud.Setup(
    x => x.Create(It.IsAny<IPOCOType>(),
        ref It.IsAny<IDatabaseType>())
    .Returns(pocoList);
```

Which you can’t do. What you have to do instead is:

``` csharp
_mockCrud.Setup(
    x => x.Create(It.IsAny<IPOCOType>(),
        ref _database))
    .Returns(pocoList);
```

And the only way this will work is if the _database variable is pointing to the same object that will ultimately get called. So, here is the magic that I used to make all that work. Reflection. The database object I needed was created as part of the constructor in yet another object that I pass into the Accept() method.  So, I create that object and then use reflection to get the instance of the database from it.

``` csharp
var visitor = new DataVisitor();
var fieldInfo = visitor.GetType()
    .GetField("_database",
        BindingFlags.NonPublic | BindingFlags.Instance);
if(fieldInfo != null)
{
    _database = (Database)fieldInfo.GetValue(visitor);
}
```

And now I can use the Setup code I’ve already specified. Hope that helps someone else.
