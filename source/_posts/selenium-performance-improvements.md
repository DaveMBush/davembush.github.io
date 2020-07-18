---
title: Selenium Performance Improvements
tags:
  - bdd
  - 'c#'
  - java
  - selenium
  - tdd
url: 2653.html
id: 2653
categories:
  - Continuous Integration
  - Selenium
  - TDD
date: 2014-10-02 06:00:00
---

As many of you know, I’ve been using [Selenium](//www.seleniumhq.org/) to do my website testing.  And, if you’ve done any testing with Selenium yourself, you know that Selenium can be even slower if you are using Selenium Grid. There are several things you might do today to achieve Selenium Performance improvements in order to increase the speed that your of your test run.

<!-- more -->

CacheLookup attribute (or Annotation if you are using Java)
-----------------------------------------------------------

This is probably the most obvious place for improvement.  Assuming you are using the Page Model pattern in your testing, your page model should have properties that already look something like:

``` csharp
[FindsBy(How = How.Id, Using = "radioButton")]
private IWebElement RadioButton{ get; set;};
```

What you may not know is that the way this works is that EVERY time you access the property named RadioButton it will lookup the location of the radio button.  The RadioButton property is actually a proxy for the real IWebElement that it will lookup on the fly. So, if you are using the element more than once in your test, it will perform the lookup each time you access the RadioButton property. By adding the CacheLookup attribute to the RadioButton property,

``` csharp
[FindsBy(How = How.Id, Using = "radioButton"), CacheLookup]
private IWebElement RadioButton{ get; set;};
```

you can force the RadioButton to be resolved once and only once. Most of my test ask for elements once per test, so I didn’t see any major performance gains by adding this.  Your mileage may vary.

CacheLookup Warning
-------------------

Be careful when adding CacheLookup to your properties because any time the HTML is recreated, the element will become “stale” and you’ll need to look it up again. For example, in one of the pages I’m testing, I have two modal windows that get created as part of the test.  Each time they are created, they have to be resolved again because the old ones no longer exist. It would be like trying to reference a pointer to an object that no longer exist because something else deleted it.  Actually, that’s exactly what is happening.

CacheLookup Only Solves One of Your Problems
--------------------------------------------

But if all you do to your site is add CacheLookup, you are wasting your time.  You have to start thinking like your code. Did you know that every time you access a property on an IWebElement, Selenium has to make a call to the browser to access the current value? Yep. Actually, if you think about it, this is what you hope it does.  It is what makes code like this work:

``` csharp
Wait.Until(x => Page.RadioButton.GetAttrinbute("disabled"));
```

But how many times to we write code that goes after a property multiple times when we know the value, or at least we assume the value, hasn’t changed? Or how many of you are doing the same thing multiple times in the same test? For example, in the code I’m testing currently, I have a header and a footer that I need to hide so that when an element is scrolled into view by Selenium, it doesn’t scroll under the header or the footer. I’m currently hiding and showing every time I perform some action on an element.  But now that I’m starting to think about performance, I’m asking myself, “What was I thinking?!” In this case, I would save a lot of time by turning the header off once a the beginning of my test.  I bet my test end up running twice as fast simply by doing that. So, keep in mind that every Selenium call you make is probably going to have to access the server in order to perform the action or retrieve the value you are looking for and try to write your code so that you are only making that call to the server one time per test. I bet you see some performance improvements.
