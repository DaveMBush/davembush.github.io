---
title: Response.Redirect() executes too soon on the Server.
tags:
  - asp.net
  - events
  - redirect
  - response
url: 788.html
id: 788
categories:
  - ASP.NET
date: 2009-01-27 05:43:45
---

![tp_vol4_017](/uploads/2009/01/tp-vol4-017.jpg) I've seen this question a couple of times in various situations. The first involves Javascript and the second involves server side code. Both are caused by a misunderstanding of what this function does and how web pages work. Let's start with the easy one: server side code.  You might have code that looks something like this:

<!-- more -->

``` csharp
protected void Page_Load(object sender, EventArgs e)
{

    // Do something here
    Response.Redirect("~/newpage.aspx");

    // do some more code here
}
```

The problem with this code, which is probably obvious to most of you, is that the "do some more code here" section will never fire because we've done a redirect right before it. But is this as obvious?

``` csharp
protected void Page_Load(object sender, EventArgs e)
{

    // Do something here
    Response.Redirect("~/newpage.aspx");

}
protected void Button1_Click(object sender, EventArgs e)
{
    // do some more code here
}
```

The problem here is that Page\_Load is the first event to fire. Button1\_Click() will never execute because we've done the redirect during our page load. Aside from the fact that you should not perform form processing code during Page_Load(), the other issue is that events can fire in any order. So maybe you were smart and processed your form in a Button.Click event handler, but have you accounted for the fact that your databinding code may need to put data in the database AFTER you run Response.Redirect()? There is a way around this. Response.Redirect() has two overloads. The first, which we are all very used to, is the one I've shown above. But all it does is call the second which has two parameters, the URL and a boolean value, to indicate if we should stop processing right away and return to the browser. The default that gets sent in with the first overload is TRUE. Go ahead and quit. By modifying our code to:

``` csharp
protected void Page_Load(object sender, EventArgs e)
{

    // Do something here
    Response.Redirect("~/newpage.aspx",false);

}
protected void Button1_Click(object sender, EventArgs e)
{
    // do some more code here
}
```

we can be sure that our Button1_Click method, or databinding code, will be executed. Tomorrow we'll look at the issues involved with Javascript.
