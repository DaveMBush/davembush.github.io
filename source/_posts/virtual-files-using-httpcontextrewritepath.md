---
title: Virtual Files using HttpContext.RewritePath()
url: 196.html
id: 196
categories:
  - ASP.NET
date: 2008-07-17 02:25:56
tags:
---

One of the cool new tricks we are able to perform in ASP.NET that we were not able to use in ASP is the ability to have ASP.NET redirect file requests in the same way that mod_rewrite does under the Apache webserver.

For example, we could have the browser ask for an image file that doesn't exist on the file system and have ASP.NET serve up the file from the database.

<!-- more -->

Or, if we wanted to protect some files, we could put them in a directory that was hard to guess and have ASP.NET load that file for us when the browser requested the file from what looked like a normal location.

The magic that makes all this work is the 404 error handler, the Application_BeginRequest() event handler, and the HttpContext.RewritePath() method.

So, the first thing we need to do is open up our Global.asax file and create an event handler for BeginRequest().

This event is the first thing that will get called when a request is made to ASP.NET.

``` csharp
protected void Application_BeginRequest(Object sender, EventArgs e)
{
}
```

Second, we will want to create a 404 error handler in IIS.

You may have done this in prior applications, like ASP, to tell the user that a page no longer existed or to redirect the browser to a page whose name had changed.

We are going to use it to force the `Application_BeginRequest()` method to fire even when a static file isn't found.

All you need to do is tell IIS that an ASPX file is the 404 handler. It doesn't matter what you call it and it doesn't matter if the file even exists on the server. The only thing that does matter is that you SAY that the file exists in the application directory that you've created your web application in.

Now that you've specified your 404 handler, any file that can't be found on the server will automatically fire the `Application_BeginRequest` event handler. So the next thing we need to do is add code in this method to check for the 404 condition.

``` csharp
if (Request.ServerVariables["QUERY_STRING"].Length > 4 &&
Request.ServerVariables["QUERY_STRING"].StartsWith("404;"))
{
  Context.Trace.Write("QueryString > 1");
  String queryString =
    Request.ServerVariables["QUERY_STRING"].Substring(4).ToLower();

  // strip off the http:// or https://
  queryString = queryString.Replace("http://", "")
    .Replace("https://", "");

  // strip of the host section
  queryString = queryString.Substring(queryString.IndexOf("/") + 1);

  string applicationDirectory = Request.ApplicationPath;
  if (applicationDirectory.StartsWith("/"))
    applicationDirectory = applicationDirectory.Substring(1);
  queryString = queryString.Substring(applicationDirectory.Length);

  if (queryString.ToCharArray()[0] != '/')
    queryString = "/" + queryString;
}
```

Remember from your ASP days, when a 404 error handler is called, the query string starts with "404;", so this code is checking for that and stripping it out. Then it looks for the file that was being requested and strips off everything but the file location. This code will work if your application is in a subdirectory or if it is in the root of your domain, leaving you with an application relative reference to the file the browser was looking for. Once you have that, you can format the request to the real location and call

``` csharp
HttpContext.Current.RewritePath(newLocation)
```

Passing in the call to an ASPX file that will actually return the correct information.

In the code that I originally wrote, this was looking for our virtual images directory and passed the call on to our virtual images handler which pulls the image out of the database.

``` csharp
if (queryString.StartsWith(imagesDirectory))
{
  HttpContext.Current.RewritePath("VirtualImage.aspx?"+queryString);
  return;
}
```

And, just to preempt any comments about performance, the VirtualImage.aspx file caches the results so that we are not constantly pulling the image from the database. Here's the full handler:

``` csharp
protected void Application_BeginRequest(Object sender, EventArgs e)
{
  if (Request.ServerVariables["QUERY_STRING"].Length > 4 &&
    Request.ServerVariables["QUERY_STRING"].StartsWith("404;")) {
      Context.Trace.Write("QueryString > 1");
      String queryString =
        Request.ServerVariables["QUERY_STRING"]
          .Substring(4)
          .ToLower();

      // strip off the http:// or https://
      queryString = queryString
        .Replace("http://", "")
        .Replace("https://", "");

      // strip of the host section
      queryString = queryString
        .Substring(queryString.IndexOf("/") + 1);

      string applicationDirectory = Request.ApplicationPath;

      if (applicationDirectory.StartsWith("/"))
        applicationDirectory = applicationDirectory.Substring(1);

      queryString = queryString
        .Substring(applicationDirectory.Length);

      if (queryString.ToCharArray()[0] != '/')
        queryString = "/" + queryString;

    string imagesDirectory =
      ConfigurationManager.AppSettings["VirtualImageDirectory"];

    if (queryString.StartsWith(imagesDirectory)) {
      HttpContext.Current.
        RewritePath("VirtualImage.aspx?"+queryString);
      return;
    }

  if (queryString.Contains("app_themes"))
    HttpContext.Current.
      RewritePath("ResolveRelativeReference.aspx?" +
      queryString);
  }
}
```
