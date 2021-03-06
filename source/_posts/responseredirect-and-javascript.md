---
title: ASP.NET Response.Redirect() and JavaScript
tags:
  - '302'
  - asp.net
  - javascript
  - redirect
  - response
url: 790.html
id: 790
categories:
  - ASP.NET
date: 2009-01-28 05:10:10
---

![A toucan perched on a branch in Brazil.](/uploads/2009/01/toco-toucan.jpg) Yesterday we covered issues surrounding using ASP.NET's Response.Redirect in server side code. We noted that not handing it correctly could prevent code from running on the server that we want to run. The other issue is emitting Javascript in the server side in association with Response.Redirect(). This also leads to unexpected problems if you aren't thinking about what is actually happening with your code.  Take this code as an example:

<!-- more -->

``` csharp
    protected void Page_Load(object sender, EventArgs e)
    {
        string myscript = @"<script language='javascript'>
alert('hello world');
</script>";
        ClientScript.RegisterClientScriptBlock
            ("".GetType(), "s", myscript);
        Response.Redirect("~/newpage.aspx",false);

    }
```

The question is, why does the javascript never display "hello world"? Actually, the javascript is typically a little more complicated than "hello world." But the question is always, "Why didn't my javascript execute? It works fine without the redirect." Once again, we need to think more clearly about what we've actually written. What we've actually told the server to do is the following:

1.  Render the javascript to display "hello world" in an alert box on the client.
2.  Set the header (not the header element, but the header that tells the browser whether the code executed successfully or not) to "302 redirect."

When the browser finally gets the stream back from the server, it will actually see step 2 first because the header comes before the javascript code. The browser will look at that 302, ignore everything else on the page, and faithfully redirect to the page specified as part of the 302.
