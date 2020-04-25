---
title: 'Testing EXTjs with Selenium using C#'
tags:
  - 'c#'
  - extjs
  - selenium
url: 3182.html
id: 3182
categories:
  - Selenium
date: 2015-07-30 06:00:00
---

![image](/uploads/2015/07/image2.png "image")

For the last two years, about 99% of the work I’ve been doing has been JavaScript programming using EXTjs.  The problems I’ve encountered along the way are primarily related to testing.  Writing unit test using the “MVC Framework” out of the box is nearly impossible, but I did eventually put together an architecture that allows me to do that.  I may blog about that in the future.

Today, what I want to offer is how I overcame various hurdles related to testing EXTjs with Selenium.  The first problem nearly everyone encounters when they first use EXTjs is that the views that are created are created dynamically.  If you use the recommended itemId to uniquely identify the elements on the page, you will be left with IDs that are dynamically generated and inconsistent.  That is, just because the ID has one value today doesn’t mean it will have the same value tomorrow.  And since the controls are dynamically created based on what browser is running, XPATH queries aren’t a good solution either.

<!-- more -->

Custom FindsBy attribute
------------------------

Now, you could solve this problem by creating a different Page Object for each browser and then you’d be able to use XPATH statements to go after the elements.  But, what if you could go after the itemIds directly?  By the Selenium APIs let you create your own selectors.  So, instead of writing your element properties like this:

``` csharp
[FindsBy(How = How.Id, Using="someId")]
private IWebElement SomeId{get; [UsedImplicitly]set;}
```

You could do something like:

``` csharp
[FindsBy(How = How.Custom,
    CustomFinderType = typeof(ExtByComponentQuery),
    Using = "#someId")]
private IWebElement SomeId{get; \[UsedImplicitly\]set;}
```

One of the side effects of using this extension is that you can now find any element using any valid EXTjs selector.

So, you are probably wondering what the code looks like to get this all working.  Well here you go:

``` csharp
using System;
using System.Collections.Generic;
using System.Collections.ObjectModel;
using System.Linq;
using System.Threading;
using OpenQA.Selenium;
using OpenQA.Selenium.Internal;

namespace ENT.Supporting
{
    public class ExtByComponentQuery : By
    {
        private readonly string _locator;
        public ExtByComponentQuery(string locator)
        {
            _locator = locator;
        }

        public static ExtByComponentQuery Query(string locator)
        {
            if (locator == null)
                throw new ArgumentNullException("locator",
                    @"Cannot find elements when name locator is null.");
            return new ExtByComponentQuery(locator);
        }

        public override IWebElement FindElement(ISearchContext context)
        {
            var elements = FindElements(context);
            if (elements != null && elements.Count > 0)
                return elements.First();
            Console.WriteLine(@"returning null from FindElement");
            return null;
        }

        public override ReadOnlyCollection<IWebElement> FindElements(ISearchContext context)
        {
            var script = string.Format(
@"var r= Ext.ComponentQuery.query('{0}');
var rarray = \[\];
for(e = 0;e < r.length;e++)
{ {
    if(r\[e\].getEl() !== undefined)
        rarray.push(r\[e\].getEl().dom);
}}
return rarray;",_locator.Replace("'","\\\'"));
            var javaScriptExecutor = context as IJavaScriptExecutor ??
                                     ((IWrapsDriver)context).WrappedDriver as IJavaScriptExecutor;
            if (javaScriptExecutor == null) return null;
            object scriptReturn = null;
            for (var i = 0; i < 120 && scriptReturn == null; i++)
            {
                if ( i > 0) Thread.Sleep(1000);
                scriptReturn = javaScriptExecutor.ExecuteScript(script);
            }
            return scriptReturn == null ? null : new ReadOnlyCollection<IWebElement>((IList<IWebElement>)scriptReturn);
        }
    }
}
```

The constructor gets passed the selector string that is passed with the Using parameter in the attribute.  The FindElements() finds all of the elements that match the selector.  The FindElement() returns the first of all that it found.

You can either use this code as part of the FindsBy attribute or you can use it stand alone by calling the static Query method.

You’ll see that the bulk of the functionality is just the same JavaScript code you would normally write in EXTjs.

Get an element’s Value
----------------------

One of the other problems you are likely to run into is retrieving the current value of an element.  This is because many of the controls that EXT provides to us do not use the INPUT item’s value to keep track of the value.  So, for this, and other similar scenarios, I wrote an extension method.

``` csharp
public static string GetExtValue(this IWebElement element)
{
    IWebElement realElement = null;
    if (element as IWrapsDriver == null)
    {
        var e = element as IWrapsElement;
        if (e != null) realElement = e.WrappedElement;
    }
    else
    {
         realElement = element;
    }

    if (realElement == null) return null;

    var wrapsDriver = (IWrapsDriver)realElement;
    var id = realElement.GetAttribute("id");
    var driver = wrapsDriver.WrappedDriver;
    var val = ((IJavaScriptExecutor)driver).ExecuteScript(
        @"return Ext.getCmp(arguments\[0\]).getRawValue();",
        id);
    if (val == null)
    {
        return string.Empty;
    }
    return val.ToString();
}
```

What you’ll want to pay attention to here is the code at the top.

The if(element as IWrapsDriver) stuff is testing to see if we have the real element or if we have a proxy to the element that was created by the FindsBy attribute.  If the element is of type IWrapsDriver, we have the proxy and so need to go grab the real element before we continue.  Once we are sure we are working with a real element, we can use JavaScript to grab the raw value from EXTjs.

Other Modifications
-------------------

I found that there were other modifications that I needed to make.  So I’ve also created extension methods that select items from dropdown list, Click radio buttons and checkboxes until they are actually selected (or actually unselected).  I found that they don’t always select or unselect on the first click for some reason.  Also, if your code is enabling and disabling stuff and you want to check for that, you’ll need to write some extensions to handle that because EXT uses a mask to disable.  The best way of determining if something is enabled or disabled is by writing JavaScript to ask EXT if it is enabled or disabled.  There are other things I’ve written that are somewhat project specific.  But if you run into a problem, your best bet is to create an extension method to go after EXT code directly.

Conclusion
----------

I generally hate using the framework I’m using to test my code from EXTjs.  And whenever possible, I’ll look for a way to go after the elements using plain old selectors and the built in Selenium code.  But that isn’t always practical in this case.
