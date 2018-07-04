---
title: Running Selenium In Parallel With Any .NET Unit Testing Tool
tags:
  - .net
  - 'c#'
  - nunit
  - parallel
  - selenium
  - testing
url: 2593.html
id: 2593
categories:
  - Selenium
  - Testing
date: 2014-07-31 13:15:00
---

Running Selenium in parallel from .NET seems to be a problem because, as of the time of this writing, I’ve yet to find a viable way of running selenium test on multiple browsers using [Selenium Grid](//docs.seleniumhq.org/docs/07_selenium_grid.jsp).  This doesn’t mean that there aren’t a few articles out there that have some kind of solution.  But they’ve never satisfied me as something that I could easily plug into my already created test. While my preferred testing tools are [NUnit](//www.nunit.org/) and [SpecFlow](//www.specflow.org/), the method I am about to propose should work with any existing test harness you might want to use.  The only prerequisite is that you are using [Page Models](//code.google.com/p/selenium/wiki/PageObjects) to wrap your access to any particular web page. This article assumes that you already:

*   know how to write Selenium tests
*   know how to use Selenium Grid
*   know how to use the Page Model pattern
*   know how to use your chosen test harness.

OK.  On to the main event.

Here Is The Problem
-------------------

In order to run multiple browsers at the same time, the easiest way is to provide a wrapper Page Model that calls multiple instances of the page model at the same time. The hard way of doing this would be to create an Interface that represented the real page model and then create a proxy class that would hold a list of all of the real page model objects we needed to call.  Each time a property or method on the proxy gets called, all it would do would be to pass the call down into the real objects in parallel. This would work, but the main draw back is that I really don’t want to have to write a method for each method in my real page model.  So the question is, how can we get around this?

DynamicObject To The Rescue
---------------------------

Enter the little known class, [DynamicObject](//msdn.microsoft.com/en-us/library/system.dynamic.dynamicobject(v=vs.110).aspx).   In .NET 4, Microsoft introduced the dynamic keyword.  One of the main uses is for places where  you need to be able to declare a variable in your code that the compiler won’t know how to resolve the type of until runtime.  I could have used this several years ago when I had two assemblies that needed to reference each other.  In that case, I used reflection.  But dynamic would have worked with a lot less work. DynamicObject is a specific class that allows us to resolve property and method calls at runtime using our own logic. We will also use the Task Parallel library to implement our parallel calls. For completeness, and so that no one is confused when they try to implement this code, you’ll need the following using statements at the top of the CS file.

Using Statements And Constructor
--------------------------------

using System;
using System.Collections.Concurrent;
using System.Dynamic;
using System.Reflection;
using System.Threading.Tasks;

So, let’s get started.  The first thing we will need is a class declaration:

class ParallelPageModel<TPage>:  DynamicObject
{
}

TPage allows us to specify the Interface the real Page Model implements.  Yes, we still need the interface, but we won’t need to create a new wrapper class for every page model we want to wrap.  The class  inherits from DynamicObject so that all of our on the fly goodness will work. Next, we’ll need some place to store an array of PageObjects we want to proxy.  So we add a private variable _page for that purpose.

private readonly TPage\[\] _pages;

By using TPage\[\], we create a variable that is the same type array as the Page Models we are proxying. Next we need a constructor.

ParallelPageModel(params TPage\[\] pages)
{
    _pages = pages;
}

By using the params keyword, we can either  pass in page objects as an array or as individual parameters. The magic happens in three overridden methods that are in DynamicObject:

*   TryInvokeMember – resolves any method calls.
*   TrySetMember – resolves any property setters
*   TryGetMember – resolves any property getters

So let’s add those methods next:

public override bool TryInvokeMember
    (InvokeMemberBinder binder, 
      object\[\] args, 
      out object result)
{
}

public override bool TrySetMember
    (SetMemberBinder binder, object value)
{
}

public override bool TryGetMember
    (GetMemberBinder binder, out object result)
{
}

TryInvokeMember
---------------

Inside of the TryInvokeMember method, the first thing we will want to do is to use reflection to call into the real methods.  Since we could have multiple instances of the same method we need to call we will want to do this in a loop. When I first worked this out, I started by just implementing a foreach loop but we are going to jump right to using Parallel.ForEach() Parallel.ForEach() will let us pass in an array and run a lambda expression on each element in the array.  So, our foreach loop will look like this:

var results = new ConcurrentBag<object>();
Parallel.ForEach(_pages, page =>
{
    var thisResult = typeof (TPage)
       .InvokeMember(binder.Name,
        BindingFlags.InvokeMethod | 
        BindingFlags.Public | 
        BindingFlags.Instance, 
        null, page, args);
    results.Add(thisResult);
});

Note that our lambda expression is not doing anything more than a simple reflection call. The result that is returned is added to our ConcurrentBag collection.  ConcurrentBag is a collection that is specifically made for parallel calls.  We could get into trouble if we added something to a List<> collection unless we added some parallelization gatekeeping around it.  I’m for doing as little work as possible. The second thing we want to do is to process the return results. For this we need to setup a basic foreach loop.

foreach (var thisResult in results)
{
}

Inside the foreach loop we will process the results collection. If the type that got  returned is the same type as the type that the page is proxying for, we just make our result value, the return value the TryInvokeMember is going to return for us to the code that called the proxy, equal to the proxy object.

if (thisResult is TPage)
{
    result = this;
}

If the result is not null, meaning either that a previous result was null or we haven’t processed the loop yet, we want to check to see if the value of the current loop result is the same as the loop results we’ve already processed.  If it isn’t, we throw an exception.

else if (result != null)
{
    if (!result.Equals(thisResult)) // not the same value
    {
        throw new Exception
           ("Call to method returns different values.");
    }
}

Finally, we just set the result to whatever we have at this point.

else
{
    result = thisResult;
}

And then the last thing we want to do is to return true to tell the system we were able to process the method.

TryGetMember
------------

Since the implementation for TryGetMember looks very similar to TryInvokeMethod we’ll tackle that next. In fact, the only difference between the two methods is the code inside of the Parallel.ForEach parameter block. So, here it is:

Parallel.ForEach(_pages, page => 
{
    var thisResult = typeof(TPage)
        .GetProperty(binder.Name).GetValue(page);
    results.Add(thisResult);
});

TrySetMember
------------

TrySetMember is the easiest implementation of all since there are no results to worry about.

Parallel.ForEach(_pages,
     page => typeof (TPage)
        .GetProperty(binder.Name).SetValue(page, value));
return true;

Casting
-------

So the code above will work, but you won’t get any intellisense help from Visual Studio if you use this code without tweaking it. What we need is some way of casting the ParallelPageModel object to the TPage type that we pass in. For that we are going to use a cool library I found called [ImpromptuInterface](//github.com/ekonbenefits/impromptu-interface). You’ll need to add a using statement.

using ImpromptuInterface;

And then you’ll need to add this method to the ParallelPageModel class.

public TPage Cast()
{
    return this.ActLike();
}

You would use this like this:

IMyPageModel p = pageModelProxy.Cast();

Where IMyPageModel is the interface that specifies what your real PageModel class looks like. Just in case someone is tempted to mention this in the comments, you can’t us operator overloading to achieve the cast because we need it to return TPage, which could be anything and the compiler can’t deal with that.  If you really want to use operator overloading you’ll need to provide your own specific implementation that ends up calling the code above.

Calling The ParallelPageModel
-----------------------------

To setup the ParallelPageModel, your code would look something like this, assuming that you have a page model class called MyPageModel with an interface of IMyPageModel.

var pages = new ConcurrentStack<IMyPageModel>();
Parallel.Invoke(
    () =\> 
        pages.Push(PageFactory.GetPageModel("FireFoxGrid"), 
    () =\> 
        pages.Push(PageFactory.GetPageModel("IE11Grid"));
var pagesArray = pages.ToArray();
MyTypedPage = 
    new ParallelPageModel<IMyPageModel>(pagesArray).Cast();

Considerations
--------------

I only just started using this.  It works for my current implementation.  But you may need to tweak it so that it works for you. For example, my assumption here is that you are only dealing with simple types or the page model type you are a proxy for.  There is no code here that will handle a situation where the call to a method would return a entirely new page model. Since the code I am testing is a collection of Single Page Applications and I am not testing navigation at this point, this is not a consideration for me.  But it would be relatively easy code to implement.  If I did that, I would probably handle it but subclassing this main class that does the bulk of the work and override the Try*Member method that needed to deal with that situation.  The other possible way of dealing with the situation is to pass in a list of types that need to be wrapped in their own parallelization object as parameters in the constructor and add some generic code in the ParallelPageModel class. Finally, I am well aware that this code may have bugs.  If you find one, go ahead and fix it. You can leave a comment so that others will benefit.   There is a [demo project on GitHub.](//github.com/DaveMBush/ParallelSeleniumUsingNUnit)

#### Other Places Talking about Parallel Selenium

*   [Using MbUnit to achieve parallelization](//slmoloch.blogspot.com/2009/12/design-of-selenium-tests-for-aspnet_19.html)
*   [Use Browser Stack](//www.browserstack.com/automate/c-sharp)

And of course a ton of links to people asking how this can be achieved.