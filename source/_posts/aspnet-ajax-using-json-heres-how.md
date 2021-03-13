---
title: ASP.NET AJAX using JSON - Here's how.
tags:
  - ajax
  - asp.net
  - json
url: 212.html
id: 212
categories:
  - ASP.NET
date: 2008-08-04 07:05:58
---

![image](/uploads/2008/08/image.png) Last week I wrote a post about [how simple JSON is](/2008/07/30/using-json-in-aspnet/).

In it I explained the main differences between using JSON and using the update panel. I really started out thinking I'd get to how to write JSON code, but I ran out of space. Well, today, we get to the HOW.

<!-- more -->

All JSON really is, is the ability to have JavaScript code call a WebService for our data, and write it into the HTML on the client side.

If you know anything about how to write a WebService, this should be rather trivial because your web service is going to look just like any other web service with the exception of an added attribute.  To make a WebService accessible to JSON, add the following Attribute to your WebService class:

``` csharp
[ScriptService]
```

You will probably also need to add

``` csharp
using System.Web.Script.Services;
```

to the top of your page.

Now, any WebMethod in your WebService can be called by your JavaScript on the client side. As with any AJAX code in ASP.NET, you'll need to add a ScriptManager to your page.

But, in addition, you'll also need to add a Services section in your ScriptManager to tell the ASP.NET page to pull in the javascript that the WebService code will now produce for you.

You should have this code at the top of the FORM section of your HTML.

``` html
<asp:ScriptManager ID="ScriptManager1" runat="server">
  <Services>
    <asp:ServiceReference path="~/WebService.asmx" />
  </Services>
</asp:ScriptManager>
```

Where, `~/WebService.asmx` refers to the web service you want to be able to access. Now in the javascript event handler where you want to be able to call the WebService, you just call the WebService similar to how you would call any other function. In fact, if you write this code in the ASPX page, you will even get intellisense. So, assuming we have a class named `WebService` with a function called "HelloWorld" that returns a string, our calling code will look like:

``` csharp
WebService.HelloWorld(Success,Fail,null);
```

Note that our call takes three parameters, even though our function in the WebService does not take any parameters.

The first parameter is a pointer to a JavaScript function to call if the call to the WebService succeeds.

The second is a pointer to the JavaScript function to call if the WebService call fails, and the third parameter is a JavaScript context object to pass to the Success function.

Since I don't have any data I need to pass along, I just pass in NULL in the code above. If the WebService method takes parameters, the parameters are listed before the success, fail, and context parameters. If the WebService call fails, the JavaScript function that we specified for fail will be called. It needs to accept one parameter which will have the error message in it. You can use this to display an error message in an alert box. When the WebService returns successfully, the Success function will be called. This function takes two parameters. The first is the return value. The second is the context object we passed when we originally called the WebMethod. You can then use standard JavaScript to find an HTML element on the page to put the new content into:

``` javascript
function Success(result,context) {
  document.getElementById("fillMeAtLoadTime").innerHTML = result;
}
```

And that's really all there is to JSON. The rest is really just a matter of imagination and JavaScript.
