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

\- LoginView  
\- LoginStatus

And the web.config file allows us to control which pages can be viewed based on which role a user is in.

But what if you need to determine the role a user is in using the APIs? How do you do that?

It turns out that the API for this is really rather straightforward.

If you are in an ASPX or ASCX file, you can use

if(User.IsInRole("roleNameHere"))
{
  // do something here
}

.csharpcode, .csharpcode pre { font-size: small; color: black; font-family: consolas, "Courier New", courier, monospace; background-color: #ffffff; /\*white-space: pre;\*/ } .csharpcode pre { margin: 0em; } .csharpcode .rem { color: #008000; } .csharpcode .kwrd { color: #0000ff; } .csharpcode .str { color: #006080; } .csharpcode .op { color: #0000c0; } .csharpcode .preproc { color: #cc6633; } .csharpcode .asp { background-color: #ffff00; } .csharpcode .html { color: #800000; } .csharpcode .attr { color: #ff0000; } .csharpcode .alt { background-color: #f4f4f4; width: 100%; margin: 0em; } .csharpcode .lnum { color: #606060; }If you are in other code where the User property is not available, you’ll need to use the HttpContext class like we’ve used previously this week to get access to the current context.

if(HttpContext.Current.User.IsInRole("roleName"))
{
  // do something here
}

.csharpcode, .csharpcode pre { font-size: small; color: black; font-family: consolas, "Courier New", courier, monospace; background-color: #ffffff; /\*white-space: pre;\*/ } .csharpcode pre { margin: 0em; } .csharpcode .rem { color: #008000; } .csharpcode .kwrd { color: #0000ff; } .csharpcode .str { color: #006080; } .csharpcode .op { color: #0000c0; } .csharpcode .preproc { color: #cc6633; } .csharpcode .asp { background-color: #ffff00; } .csharpcode .html { color: #800000; } .csharpcode .attr { color: #ff0000; } .csharpcode .alt { background-color: #f4f4f4; width: 100%; margin: 0em; } .csharpcode .lnum { color: #606060; }
