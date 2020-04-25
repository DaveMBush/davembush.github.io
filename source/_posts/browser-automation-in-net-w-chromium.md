---
title: Browser Automation in .NET w/ Chromium
tags:
  - 'c#'
  - CefSharp
  - Chromium
url: 3153.html
id: 3153
categories:
  - 'c#'
date: 2015-07-09 06:00:00
---

![image](/uploads/2015/07/image.png "image") Over the past ten years, I’ve successfully implemented various types of screen scraping in order to provide data to my clients.  Most of these implementations have involved accessing HTML and parsing out the data we needed for the web application.

My latest implementation of this made use of the [HTML Agility Pack](//htmlagilitypack.codeplex.com/) and managed to incorporate the e-Labels For Education site into the Labels For Education site.  (No links, because the e-Labels program is being phased out.) Recently, I’ve been spending a lot of times on some site doing the same thing over and over again.  But most of the sites I visit now implement some kind of AJAX so that doing a simple web request to a page without also loading and parsing the JavaScript ends up giving me a page with no useful data at all.  Unlike the work I’ve done in the past where this was sufficient.

This, combined with my recent work implementing Jasmine unit test for JavaScript and running them in the PhantomJS headless browser has had me thinking, wouldn’t it be great if I could do similar kinds of screen scraping, or even browser automation, but use something like an embedded version of PhantomJS to get the work done.

<!-- more -->

Well, do a search for “embedded PhantomJS for .NET” or something similar and you’ll find that that isn’t possible.  At least not yet.

But there is a viable alternative.  Actually there are a couple of viable alternative.  But they all end up using the Google Chromium browser API.  The implementation I ended up using is [CefSharp](//github.com/cefsharp).  Mostly because this is what is available from NuGet.

What follows are some of the tricks I learned along the way.

Installing Chromium
-------------------

The first thing you’ll need to do once you have a project started, is that you’ll need to install the Chromium DLLs.  In the NuGet package manager, do a search for CefSharp.  This will bring up a list of plugins, you’ll want to install CefSharp.OffScreen and CefSharp.Common (I’m assuming you want to do off-screen automation here.)  The version I am working with is version 39.

What I found difficult to figure out next was how to actually use the library.

Get It Initialized
------------------

You’ll want to initialize the library before you use it.  The following lines will do that.  I just put this as early in my code as possible.

``` csharp
var settings = new CefSettings
    {LogSeverity = LogSeverity.Verbose};
settings.CefCommandLineArgs.Add("no-proxy-server", "1");

Cef.OnContextInitialized = ()
    =\> Cef.SetCookiePath("cookies", true);

if (!Cef.Initialize(settings, shutdownOnProcessExit: false,
    performDependencyCheck: true))
{
     throw new Exception("Unable to Initialize Cef");
}
```

If you want to use a proxy server, you’ll need to look up the documentation for how to set the proxy server.

The Cef.SetCookiePath sets the location of your cookie file.

Creating The Browser “Window”
-----------------------------

Now that you have this all set, you can use the ChromiumWebBrowser class to create a browser window.  Since the browser is disposable, you’ll want to either wrap the code in a using() statement or you’ll want to make sure you dispose of the browser object when you are done.

You’ll want to set a few things on the browser object next.

``` csharp
browser.BrowserSettings
    .FileAccessFromFileUrlsAllowed = true;
browser.BrowserSettings
    .UniversalAccessFromFileUrlsAllowed = true;
browser.BrowserSettings
    .WebSecurityDisabled = true;
```

And then you’ll want to wait for the browser to initialize.

Now the code I was given for this looks like this:

``` csharp
public static Task WaitForBrowserToInitialize(
    this ChromiumWebBrowser browser)
{
    var tcs = new TaskCompletionSource<bool>();

    EventHandler handler = null;
    handler = (sender, args) =>
    {
        browser.BrowserInitialized -= handler;
        tcs.TrySetResult(true);
    };
    browser.BrowserInitialized += handler;

    return tcs.Task;
}
```

You’ll recognize this as an extension method.  What it is doing is waiting for the BrowserInitialized event to fire and then telling the task it can return.  This works great the first time you use it, but I found that when I created a new browser “window” the initialization happened so quickly that this was unreliable.  I’ve replaced this code with the more reliable version below.

``` csharp
public static Task WaitForBrowserToInitialize
    (this ChromiumWebBrowser browser)
{
    while (!Browser.IsBrowserInitialized)
    {
        await Task.Delay(100);
    }
}
```

It does the same thing.  It is just more reliable.

Load a Page
-----------

Everything else is pretty straight forward.  To load a web page:

``` csharp
public static Task LoadUrl(
    this ChromiumWebBrowser browser, string url)
{
    browser.Load(url);
    return browser.WaitForPage();
}
```

That WaitForPage() method looks like this:

``` csharp
public static Task WaitForPage(
    this ChromiumWebBrowser browser)
{
    var tcs = new TaskCompletionSource<bool>();
    EventHandler<NavStateChangedEventArgs> handler = null;
    handler = (sender, args) =>
    {
        //Wait for while page to finish loading not
        // just the first frame
        if (!args.IsLoading)
        {
            browser.NavStateChanged -= handler;
            tcs.TrySetResult(true);
        }
    };

    browser.NavStateChanged += handler;
    return tcs.Task;
}
```

Get Data Out
------------

If you need to get data out of the page, you can use GetSourceAsync();

``` csharp
var source = await browser.GetSourceAsync();
```

or you can use JavaScript to get at the DOM using

``` csharp
await EvaluateScriptAsync(javaScriptCodeHere);
```

Note, you can also use `EvaluateScriptAsync` to do things like clicking buttons, scrolling the window and a lot of other useful things.

### Other Places Talking About Chromium for .NET

*   [Embed Chromium Using CefGlue](//umaranis.com/2013/10/16/how-to-embed-chrome-browser-in-net-application/)
*   [Embedded Chromium in WinForms](//thechriskent.com/2014/08/18/embedded-chromium-in-winforms/)
