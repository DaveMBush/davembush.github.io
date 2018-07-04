---
title: 'C# Self Executing Anonymous Function'
tags:
  - anonymous
  - function
  - method
  - self executing
url: 2132.html
id: 2132
categories:
  - 'c#'
date: 2013-02-28 10:57:00
---

[![arct-059](/uploads/2013/02/arct-059_thumb.jpg "arct-059")](/uploads/2013/02/arct-059.jpg)Not because it is all that new, but because it took me a while to find it, here’s how to create a self executing anonymous function using CSharp, just like you can do in JavaScript. ((Func<int>)(delegate() { return 0; }))();    Func<int\> says the the delegate will return an integer.  If you need to pass a parameter you can use the alternate form of Func<parameterType,returnType> delegate() {return 0;} is just basic [delegate syntax](/delegates-in-net/) that I’ve discussed in the past.  The parentheses around everything just force the order of operation so the compiler can deal with it all. And here is the code that I needed this for

list = schools.Select(school => new SchoolItem
{
... code omited ...
    Percentage = ((Func<int>)(delegate()
    {
        var r = 0;
        var found = collectionInOuterScope
             .Find(x => x.ID == school.Id);
        if (found != null)
        {
            r = found.Percentage;
        }
        return r;
    }))()
}).ToList();

Yes, I could have just created a function and called it, but it hardly seems worth it when I’m going to call it in one place.
