---
title: Determine The Role of a User in ASP.NET
tags:
  - asp.net
  - authentication
  - 'c#'
  - isinrole
  - roles
  - user
url: 1466.html
id: 1466
categories:
  - forms authentication
date: 2009-10-13 06:46:21
---

![winter-016](/uploads/2009/10/winter016.jpg "winter-016")

There are several controls that allow you to display content based on the role a user is in, including:

- LoginView
- LoginStatus

And the web.config file allows us to control which pages can be viewed based on which role a user is in.

But what if you need to determine the role a user is in using the APIs? How do you do that?

<!-- more -->

It turns out that the API for this is really rather straightforward.

If you are in an ASPX or ASCX file, you can use

``` csharp
if(User.IsInRole("roleNameHere"))
{
  // do something here
}
```

If you are in other code where the User property is not available, you’ll need to use the HttpContext class like we’ve used previously this week to get access to the current context.

``` csharp
if(HttpContext.Current.User.IsInRole("roleName"))
{
  // do something here
}
```
