---
title: 'Create A Desktop Application using Angular, Bootstrap and C#'
tags:
  - angular
  - angular.js
  - bootstrap
  - javascript
url: 3252.html
id: 3252
categories:
  - Javascript
date: 2015-10-15 07:30:00
---

Last week I mentioned that it is possible to [create a desktop application using JavaScript](/7-reasons-every-programmer-needs-to-learn-javascript/) and that I had actually started working on an application that used Angular and Bootstrap for the presentation layer.  I actually have enough of that working that I can share the “how-to” with you.

![image](/uploads/2015/10/image1.png "image")

<!-- more -->

Why Would You Do This?
----------------------

Well, I don’t know why YOU would do this, but the reason I’m doing this is because the more I do on the web, the less able I am to work with Windows Form, and I haven’t even bothered learning WPF.  I decided several years ago that I would niche down over web technologies.  And yet, I want to write this desktop application.  I tried to use Windows Form, which I am most familiar with, and just got frustrated.  I want to use a grid control.  But what I want to do with the control is something more like what I would do with Angular’s ui-grid than what I can do with the grid control built into Windows Form. I’m sure someone who really knew the desktop side of the fence would be able to do what I want to do.  But I want to leverage what I know.  And eventually, I may move the whole thing to Node.js even though to get the thing up and running, I am going to use C# for the main processing.

Rendering HTML
--------------

The first step toward getting all of this working is to just get HTML to render inside of a Windows Form (or WPF) executable.  I decided to use Windows Forms because I don’t need any of the WPF goodness that WPF would give me.  But you could tweak most of the setup I’m going to give you so that it would work with WPF if that’s your preferred platform.

So, let’s start out by creating a Windows Form based application.  Once you have the project loaded, you’ll want to grab the [CefSharp Windows Forms DLL’s and related files](//www.nuget.org/packages/CefSharp.WinForms/).  You can use NuGet to get these installed.  Just search for, “CefSharp.WinForms”.

Because chromium uses Win32 or Win64 based C++ DLLs, you’ll need to configure your project to run as one or the other project.  This part was a little tricky.  What I found was that just changing the project settings for the default configuration named “Any CPU” was not enough.  What you need to do is to create a new project named “x64” or “x32” and change the settings there.

Try compiling now, before you add any code.  If you’ve configured the project correctly with the CefSharp DLLs it should compile.

The next thing you want to do is to insert the Chromium Browser control into the form.  Yes, it is a control like any other control.  No, you won’t find it on your toolbar.  No, it isn’t worth adding to the toolbar.  It is the only control that is going to be on the form so all you need to do is add it to the form using a few lines of code.

First, add a private variable to hold the browser control.  It doesn’t need to be a member variable to get the HTML to render, but you’ll want it to be private later on.  So, just make it private to start with.

Then, in your Load() method, add the following code:

``` csharp
private void Form1_Load(object sender, EventArgs e)
{
    // Initialize CefSharp
     Cef.Initialize();

    // Create a new browser window
    _browser =
        new ChromiumWebBrowser("http://www.google.com/");

    // Add the new browser window to the form
    Controls.Add(_browser);
}
```

You will also need code in your `FormClosing()` method.  You can create this in Visual Studio by selecting it from the dropdowns in the upper right corner of the code window.

``` csharp
private void Form1_FormClosing(object sender, EventArgs e)
{
    Cef.Shutdown();
}
```

OK.  Compile and run.  You should be able to load the Google web site and see it in your Windows Form.

Using Our Own Files
-------------------

OK, so we’ve proven that we can render HTML inside of a Windows Form application.  But that won’t do us much good if we want to run code on our own.  Most of the places on the web that talk about loading HTML inside of a desktop application using Chromium suggest that you copy the HTML files over as content and use the file:// protocol to load them.  But there are two problems with doing that.  First, I don’t want the files generally accessible to whoever has this installed.  What if someone decides to change those files? The second problem I have is even worse.  Assuming I could live with the files being available on the file system, Angular doesn’t work from the file system.  It wants to run from http://somedomain/.  So at the very least, we need for our files to LOOK like they’ve been served from a web server.

Fortunately, we can solve both of these problems.

### Make Our Files Resources

To start with, we’ll just add one file.  Since it will be the beginning of our main application, name the file index.html and place it in a directory called “web” off the root of your project.  Put enough HTML in there that you’ll know the file actually got loaded.

Then in the file properties, mark the file as an “Embedded Resource” instead of “Content” To load this file as a resource, you’ll use code that looks something like this:

``` csharp
var assembly = Assembly.GetExecutingAssembly();
var textStream = assembly.GetManifestResourceStream
                 ("TopLevelNamespace.web.index.html");
```

### Make it LOOK Like it Came From a Server

This is where some of the magic starts to happen.  The Chromium APIs have code that will let you register a pre-canned response object with a URL using a dictionary.  So, all we need to do is change the text string that we returned in the code above into a response object and register it with Chromium.

The code to do that looks like this:

``` csharp
var factory = (DefaultResourceHandlerFactory)(browser.ResourceHandlerFactory);
if (factory == null) return;
var response = ResourceHandler.FromStream(textStream);
factory.RegisterHandler("http://local/", response);
```

And now, when we tell Chromium to load “http://local/” it  will render the index.html file from our EXE.

Cool! Now, loading each file like this is going to get rather tedious pretty fast.  So what we need is a mechanism for loading all of the files in our web directory automatically.  For this we need to be able to iterate over all of our resources in the web namespace and register them with an associated “http://” tag.

Since the best that we can do is get a list of all of the resources in our assembly, we will have to do some filtering to only register stuff in the “web” namespace.  But, there is another issue.  All of the resources are going to be listed as “TopLevelNamespace.web.subnamespace.filename.extension” and we want to register them as “http://local/subnamespace/filename.extension”.  So there is a bit of string manipulation that we need to go through to register everything correctly.

``` csharp
// Get the list of resources
var resourceNames = Assembly.GetExecutingAssembly()
    .GetManifestResourceNames();

// For each resource
foreach (var resource in resourceNames)
{
    // If it isn't in the "web" namespace, skip it.
    if (!resource.StartsWith("TopLevelNamespace.web"))
        continue;

    // Strip out the namespace that we don't need.
    var url = resource.Replace
        ("TopLevelNamespace.web.", "");

    // Function I made that turns the
    // resource into a textStream
    var r = LoadResource(url);

    // Make the namespace look like a path
    url = url.Replace(".", "/");
    var lastSlash = url.LastIndexOf("/",
        StringComparison.Ordinal);
    url = url.Substring(0, lastSlash) + "." +
        url.Substring(lastSlash + 1);

    // Register the response with the URL
    factory.RegisterHandler("http://local/" + url,
        ResourceHandler.FromStream(r));
}
```

Now that I’ve explained all of the code.  The full class for loading the resources looks like this:

``` csharp
class RegisterWebsite
{
    public static void Load(ChromiumWebBrowser browser)
    {
        var factory = (DefaultResourceHandlerFactory)
            (browser.ResourceHandlerFactory);
        if (factory == null) return;
        var response = ResourceHandler
            .FromStream(LoadResource("index.html"));
        factory.RegisterHandler("http://local/", response);

        var resourceNames = Assembly.GetExecutingAssembly()
            .GetManifestResourceNames();
        foreach (var resource in resourceNames)
        {
            if (!resource.StartsWith("TopLevelNamespace.web"))
                continue;
            var url = resource.Replace("TopLevelNamespace.web.", "");
            var r = LoadResource(url);
            url = url.Replace(".", "/");
            var lastSlash = url.LastIndexOf("/",
                StringComparison.Ordinal);
            url = url.Substring(0, lastSlash) + "." +
                url.Substring(lastSlash + 1);
            factory.RegisterHandler("http://local/" + url,
                 ResourceHandler.FromStream(r));
        }
    }

    static Stream LoadResource(string filename)
    {
        var assembly = Assembly.GetExecutingAssembly();
        var textStream = assembly
            .GetManifestResourceStream("TopLevelNamespace."
                + filename);
        return textStream;
    }
}
```

There is some obvious room for improvement here.  But the basics are there, you can tweak as needed.

The main entry point is the Load method where we pass in a pointer to the browser control we created when we started this project.

Getting JavaScript to talk to C#
--------------------------------

Now that we have the basics out of the way, we need to get the two halves of our project talking to each other.  The first half is that we need a way for our JavaScript client side code to retrieve data and send notifications to our server side code.  Fortunately, the mechanisms for doing this are already built into Chromium.

Any C# object can be registered with Chromium as a JavaScript object so that any property will become a JavaScript field and any method will become a JavaScript method.

The API to make this happen looks like this:

``` csharp
_browser
    .RegisterJsObject("NameYouWantJavaScriptToSeeThisObjectAs",
        cSharpObjectHere);
```

In our JavaScript code, we would find that the window object now has a field named “NameYouWantJavaScriptToSeeThisObjectAs”

Getting C# to talk to C#
------------------------

The reverse is just as easy.

_browser.ExecuteScriptAsync(string) takes a string that is the JavaScript that you want to execute.

Getting the Communication To Play Nice with Angular
---------------------------------------------------

But getting this all to play well with [Angular](//angularjs.org/) requires just a little bit more.

You may find that code on your screen that depends on a field or method that was registered with RegisterJsObject does not update when it should.  In fact, I would guess that this would happen most of the time because our C# object knows nothing of Angular and Angular knows nothing of our C# object.  So to fix this, we will need to make sure we $watch our C# object in our angular code.

``` javascript
$scope.$watch(function()
    {return window.RegisteredObject.property},
    function(){
    $scope.someField = window.RegisteredObject.property;
});
```

What this code does is that it tells Angular to check this field when it goes through its $digest cycle.  If it has changed since the last time it looked, it should run the second function that was passed in to $watch().

But this isn’t the only code you will need to add.  Whenever you make a change to something on the C# side that the Angular code needs to reflect, you’ll need to tell Angular to run the $digest() cycle manually.  To do that, you’ll use that ExecuteScriptAsync() method to run some JavaScript.

The easiest way to do this is to just run it off the top level $scope object.  The way you find the top level $scope object is to use JavaScript to find the element that you marked as “ng-app” in your HTML.  Once you’ve done that, you will see that it has a scope() method hanging off of it.  So this code will force a $digest cycle on everything from the top level $scope all of the way down.

``` csharp
_browser
    .ExecuteScriptAsync("angular.element('[ng-app]').scope().$digest();");
```

Alternatively, you could skip setting the watch and have your ExecuteScriptAsync call set the $scope variables directly using something like this:

``` csharp
_browser.ExecuteScriptAsync(
  "angular.element('#IdOfViewThatHasAControllerAttached')."+
  "scope().status = 'this is a new status';angular." +
  "element('[ng-app]').scope().$digest();");
```

Where #IdOfViewThatHasAControllerAttached is an ID of a element in a view that you’ve associated with a controller.  You’ll still want your controller to pull from the C# JavaScript object for the initial load because the DIV may or may not be there when you do the push.  Personally, I prefer the $watch method.  There is less to think about on the C# side.

And that’s how you create a desktop application using Angular, Bootstrap and C#.
