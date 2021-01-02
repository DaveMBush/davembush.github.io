---
title: Forms Authentication – Manual Authentication
tags:
  - authentication
  - formsauthentication
  - manual
  - redirectfromloginpage
  - setauthcookie
url: 1455.html
id: 1455
categories:
  - forms authentication
date: 2009-10-05 07:45:05
---

![F03I0043](/uploads/2009/10/F03I0043.jpg "F03I0043")

I’ve had several occasions in the past where I’ve needed to do my own authentication or I’ve needed to add some additional methods to the authentication process.

As easy as Microsoft has made the authentication process, you might think that in order to manually authenticate you’d need to write all of your authentication code manually.  But nothing could be farther from the truth.

<!-- more -->

In fact, most of the time all you need to do is trap an event handler in the existing login control.

A couple of years ago, I was asked to create a login page that used a web service to authenticate the user.  I also needed to add another form field to the login screen, so it became obvious that to do this I’d need to turn the login control into a templated control.

Once this was done it was a simple matter to trap the click event of the login button, authenticate against the web service, and then set the authentication cookie for ASP.NET.

Since I can’t show you how to authenticate against the service--your implementation will almost certainly be different--we will skip that section.  But to set the cookie, all we need to do is to revert to the ASP.NET 1.1 way of setting up the login.

```csharp
if (Request.QueryString["ReturnUrl"] != null)
{
    FormsAuthentication.RedirectFromLoginPage
        (m_tbUsername.Text, persistentCookie);
}
else
{
    FormsAuthentication.SetAuthCookie
        (m_tbUsername.Text, persistentCookie);
    Response.Redirect("~/");
}
```

The first line checks to see if there was a Return URL specified.  If there was we can use the RedirectFromLoginPage API.  Otherwise, we need to set the Authentication Cookie manually and redirect on our  own.

The persistentCookie parameter is true if we want the user to always be logged in.  Otherwise, the login is for the session.
