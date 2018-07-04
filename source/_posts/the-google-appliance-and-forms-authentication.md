---
title: The Google Appliance and Forms Authentication
tags:
  - forms authentication
  - ga
  - google
  - search
url: 1883.html
id: 1883
categories:
  - ASP.NET
date: 2010-08-04 07:37:57
---

![iStock_000003551835Medium](/uploads/2010/08/iStock_000003551835Medium.jpg "iStock_000003551835Medium") I’ve been working with a client to implement the Google Appliance on one of their sites that has forms authentication enabled. For those of you who aren’t aware, Google provides a box that you can install to index your own site using essentially the same logic that Google uses to index the Internet.  The advantage is that you have a lot more control over what gets indexed and when it gets indexed than if you just use what Google provides from its public index of the Internet.  The problem we had was that Google is not particularly fond of ASP.NET Forms Based authentication.  We had a contact at Google who was supposed to help us get this working, but it just doesn’t. What made our implementation even trickier, and worth pursuing the Google way, was that depending on who you are you shouldn’t be able to see some of the content.  So it wasn’t just a matter of getting the site indexed, the results had to be different depending on who was logged in.  We got past this by just indexing the site into a different database for each type of user.  We switch which database we look at based on the role of the user. But there was still the matter of getting the index built in the first place.  We had tried providing an alternate form that Google could use, but for whatever reason, that didn’t seem to work. What we ended up doing was setting a cookie at the Google Appliance that the the site would look at during the request process, specifically during the Application_AuthenticateRequest event.  I put the code in Global.asax file, but you can create a handler for it if you want. Here’s the code:

void Application_AuthenticateRequest(object sender, EventArgs e)
{
    HttpCookie  o =
       HttpContext.Current.Request.Cookies\["secretName"\];
    if (o != null)
    {
        System.Security.Principal.GenericIdentity id =
            new System.Security.Principal
               .GenericIdentity(o.Value);
        Context.User =
            new System.Security.Principal
                .GenericPrincipal(id, null);
    }
}

All this does is look for the cookie and set the current user name to the value that is in the cookie.  This is enough for .NET forms authentication to think the user is logged in. Our role information is retrieved using a property in a class so in the code that looks to see what role the user is a part of, we look at the username and hard wire the role based on that user. Hope this helps someone else save some time implementing this. **Other places talking about Forms Authentication and the Google Appliance** [Google Search Appliance: Benefits for enterprise content ...](//blogs.perficient.com/ecm/blog/2009/12/21/google-search-appliance-benefits-for-enterprise-content/) \- Integrate with LDAP, NTLM, Windows Integrated Authentication, forms-based single sign-on security systems, including Oracle Access Manager and CA SiteMinder, to enable seamless secure searching. Perficient has been a Google Enterprise ...
