---
title: WebServices – Error Handling
tags:
  - asmx
  - error handling
  - exceptions
  - webservice
url: 1181.html
id: 1181
categories:
  - ASP.NET
date: 2013-12-11 19:20:26
---

![tp_vol2_004](/uploads/2009/06/tp-vol2-004.jpg "tp_vol2_004") Several weeks ago I presented jQuery at the DotNet User’s Group in Connecticut.  As part of that presentation, I mentioned that I handle errors from my WebServices in a slightly different way than what most authors teach.

I thought this morning would be a good time to cover that method in detail.

As I mentioned in the presentation, I do not make use of the SoapException stuff that is part of the SOAP standard for WebServices.  This is because rolling my own SOAP Exception is a lot of work for not a whole lot of pay back and it doesn’t give me the information I’m really looking for when an exception occurs on the server side.  I also don’t let .NET wrap the exception for me because by the time the error gets back to the client, it has even less information than if I had rolled my own.

This is not to say that you shouldn’t handle the SOAP Exception on the client side.  There are several reasons why a SOAP Exception might be thrown that have nothing to do with what happened inside the method, but once I’ve successfully made a call into my method, I’m going to handle the exceptions that happen inside the method with my own exception handlers and reporting.

All I do is simply create a structure that has two elements in it.  The data I want to return from the web service and a variable I name “err” that is a string that holds the error message from the exception that was thrown.  So if my web service returned an Integer, my structure would look something like this:

public struct RetValue {
    public int value;
    public string err;
}

[](//11011.net/software/vspaste)and then I would use it in my code like this

\[WebMethod\]
public RetValue Add(int a, int b)
{
    RetValue r;
    try {
        r.value =  a + b;
    }
    catch (Exception e)
    {
        r.err = e.Message;
    }
    return r;
}

[](//11011.net/software/vspaste)

Obviously, for a real production system, you’d do more with the error handling.  This is just a demo.

You might also consider adding the callstack to the structure and returning that so that you can tell where the error came from.  You might do that only in debug mode and leave the err for release mode.

I’ve been using this method for years.  It might not be the “sanctioned” way of handling errors, but I think it is a lot easier to deal with.
