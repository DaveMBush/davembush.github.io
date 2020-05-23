---
title: Dependency Injection Frameworks Are NOT Dependency Injection
tags:
  - dependency injection
  - tdd
  - test driven development
  - testing
url: 2977.html
id: 2977
categories:
  - Dependency Injection
date: 2015-03-12 07:00:00
---

![land-0148](/uploads/2015/02/land-0148.jpg "land-0148")

As you start your journey down the road of Unit Testing you will discover that part of what makes code testable is this concept of [Dependency Injection](//en.wikipedia.org/wiki/Dependency_injection "Dependency injection").  As you explore further, you will see people mentioning various Dependency Injection frameworks.

You may naturally assume that to implement Dependency Injection, you will need to select an use a Dependency Injection framework.

But, Dependency Injection has nothing to do with using a Dependency Injection framework.  The frameworks are there because:

1.  much of our existing code is code that has too many dependencies and the framework helps us break those dependencies without having to refactor too much of our code and
2. to give us a way to easily swap out one object for another when our code is structured in such a way as to not have dependencies at all.

<!-- more -->

So, what then is Dependency Injection?
--------------------------------------

I once heard this maxim that explains it best,

> Classes should either create stuff, or do stuff, but no one class should do both.

Much of our code looks something like this:

``` csharp
private static void ReceiveSAMLResponse(
    out SAMLResponse samlResponse,
    out String relayState)
{
    // Receive the [SAML](//en.wikipedia.org/wiki/Security_Assertion_Markup_Language "Security Assertion Markup Language") response over the
    //specified binding.
    XmlElement samlResponseXml;

    ServiceProvider.ReceiveSAMLResponseByHTTPPost(
        HttpContext.Current.Request,
        out samlResponseXml, out relayState);
    SAMLResponse resp = new SAMLResponse(samlResponseXml);
    XmlElement samlAssertionElement =
        resp.GetSignedAssertions()\[0\];

    // Verify the response's signature.
    XmlDocument doc = new XmlDocument();
    //metadata file path (holds the description key).
    doc.Load(HttpContext.Current.Server.MapPath("~/SAML.xml"));
    XmlElement root = doc.DocumentElement;

    if (!SAMLAssertionSignature.Verify(
        samlAssertionElement, ReadMetadata(root)))
    {
        samlResponse = null;
        relayState = null;
        return;
    }
    // Deserialize the XML.
    samlResponse = new SAMLResponse(samlResponseXml);
}
```

Yes, this is real code from a system I worked on.  The original code was written three or four years ago and this specific code is code I was given by another company.  I just used copy and paste inheritance to get it working in our code.

That’s not to say I haven’t written code that has just as many problems.

There are several things that are wrong with this code, but for now all I want to focus on is the Dependency Injection issue.

All this code is trying to do is to deserialize the encrypted samlResponse object that was posted to the login form.  At least this code isn’t in the login form!  It has that much going for it.

But here are places where it is dependent on too much:
------------------------------------------------------

*   ServiceProvider is a static class and called directly.
*   ReceiveSAMLResponseByHTTPPost is dependent on the Request object that we retrieve from HttpContext.
*   I create a new SAMLResponse object right before I call GetSignedAssertions()
*   I create a new XmlDocument object so I can load the SAML.xml file
*   SAMLAssertionSignature is static and called directly

In fact, this method is one huge dependency mess.  And I’m very aware of the mess that it is in because the company we wrote this code for just recently switched providers.  As we tried to get this working, I had no way of testing this code in isolation without adding code directly into this method.  We got it working, but it didn’t have to be that hard.

So, here’s what I’d do to this code.
------------------------------------

*   Since ServiceProvider and SAMLAssertionSignature are calls to a third party API, I would wrap them in a none static class that I can instantiate.
*   I would have the class that ultimately calls this method either pass in the dependent objects to the constructor, pass them in to the method that calls this, or set properties in the class.  This is what it means to inject dependencies.
*   I would look for some way to avoid creating a new SAMLResponse object.  I’m not looking at the API documentation, but it would be great if I could new up the object and call a method, or set a property to give it the samlResponseXml variable.  If I had to, I’d probably wrap SAMLResponse in another class so that I could gain this functionality.
*   Finally, I would make each of the dependent object based on an interface so that I could swap them out.  In the case of some of these classes, I may need to wrap them in another class that I have control over so that I can implement an interface.  For example, as things stand, I would not be able to create a fake request object because Request is a system class that does not implement an interface.

Advantages
----------

After making all of these changes, I would be able to create a test harness for this code, create fake versions of the objects, and verify that this method does what we intend for it do.

Where this would have been particularly helpful is over this last week when we were trying to get this all working with the new provider.  In that case, I could have faked out the request object with what they were sending us and run it through a debugger to figure out what wasn’t quite right.

No Dependency Injection Framework
---------------------------------

Finally, you’ll notice that no where in this code did I have to use a Dependency Injection framework to get this all working.
