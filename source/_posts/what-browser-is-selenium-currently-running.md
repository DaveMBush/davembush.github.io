---
title: What Browser Is Selenium Currently Running
tags:
  - automation
  - browser
  - 'c#'
  - selenium
  - testing
url: 3159.html
id: 3159
categories:
  - Selenium
date: 2015-07-16 06:00:00
---

![ppl-kid-044](/uploads/2015/07/ppl-kid-044.jpg "ppl-kid-044")

This probably doesn’t happen all that often, but this last week I came across the need to know which browser I was running my selenium test against.  I figured that buried deep in the object structure of Selenium, there MUST be a way of finding out what browser I was currently running.  As it turns out, I was right.  The situation that caused this requirement is that the place where I’m currently writing most of my code is upgrading all of the Firefox browsers.  I was asked to make sure the code we were running still works with the new browser.  When I did, I found that some of my tests broke even though the test itself still succeeds when it is run manually.  In fact, they only broke when running against this new version of Firefox.

<!-- more -->

Now to be clear, this isn’t really a Firefox problem.  In fact, the new version of Firefox runs my test the way I would expect.  It is the current version of IE and the older version of Firefox that we were running in combination with EXTjs 4.2 that was a problem.  So, now I’m in a situation where I need to run my work around for everything except for this new version of Firefox.

You see, we have several dropdown list.  To select an item from the list, I cursor down until the correct item is found.  My old implementation actually needed to cursor down twice.  The first time made the list display and kept the current item selected.  The second down arrow selected the next item.  The tab key keeps that item as the selected one.  The new version of Firefox only requires one down arrow and a tab to do the same thing.

Since we have multiple dropdowns that behave the same way, I made this code an extension method, so I only have to do it once.  The other thing I’m doing that makes this work is that I’m proxying the elements using the FindsBy attribute.

``` csharp
[FindsBy(How = How.Id, Using = "someIdHere"), CacheLookup]
private IWebElement SomePropertyName
    { get; \[UsedImplicitly\] set; }
```

Under the hood this creates a proxy to the real element.  What we need access to is the Driver object that this element is using to get at the element on the browser.  I do that by going after the element’s WrappedDriver property.  In most code you’ll see this cast to an IWrapsDriver interface.  But it is actually pointing to a RemoteWebDriver object.  If we cast the type to that, we can then go after the Capabilities object which will give us access to the BrowserName. And so, my resulting code looks something like this:

``` csharp
var keyDown = Keys.Down + Keys.Down;
var wrapsDriver = ((IWrapsDriver)(element)).WrappedDriver;
if (wrapsDriver != null)
{
    if (wrapsDriver.GetType() == typeof(RemoteWebDriver) &&
        ((RemoteWebDriver)wrapsDriver)
        .Capabilities.BrowserName ==  "firefox")
     {
        keyDown = Keys.Down;
     }
}
```

Which allows me to use the keyDown variable where ever I need to key down to select an item from the list.

The element variable above represents the proxy element created by the FindsBy attribute.  The code is in the extension method that I mentioned previously, so it is actually the parameter that got passed into the extension method.
