---
title: ASP.NET Application_Error Detecting 404's
tags:
  - '404'
  - application_error
  - asp.net
  - exceptions
url: 854.html
id: 854
categories:
  - ASP.NET
date: 2009-03-02 05:56:18
---

![misc_vol3_046](/uploads/2009/02/misc-vol3-046.jpg) For many of you, this is going to be a "Duh!" kind of post.  But while working on this today, I found so many people asking this question and so many others giving the wrong answer, I'm compelled to post anyhow. If you know the answer, then you are welcome to stop reading now.  I didn't write this for you.  I wrote this for the hundreds of people who will search for this information and won't be able to find the answer.  The fact of the matter is, that's why I write most of what I write--so people searching for the information can find it.  So here's the question: I've set up an Application_Error event handler in my Global.asax file and I have implemented a server transfer for errors.  Now I want to set up a specific page to handle 404 errors.  How do I detect a 404 error and call the 404-specific page? The main answer to this question involves retrieving the exception that triggered the event in the first place.  To do that, we call `Server.GetLastError()`:

``` csharp
Exception ex = Server.GetLastError();
```

What we need to do next is determine if the exception is an HttpException or something else.  Once we have determined that it is an HttpException we will have access to Http-specific properties and methods that will give us the rest of the information we are looking for.  In our case we want to call the GetHttpCode() method, which will return the Http status code and compare it to the number 404. Our resulting code looks like this:

``` csharp
void Application_Error(object sender, EventArgs e)
{
    Exception ex = Server.GetLastError();
    if (ex is HttpException)
    {
        if (((HttpException)(ex)).GetHttpCode() == 404)
            Server.Transfer("~/Error404.aspx");
    }
    // Code that runs when an unhandled error occurs
    Server.Transfer("~/DefaultError.aspx");

}
```

That's all there is to it.
