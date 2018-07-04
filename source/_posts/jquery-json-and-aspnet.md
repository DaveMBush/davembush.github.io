---
title: 'jQuery, JSON, and ASP.NET'
tags:
  - asp.net
  - jQuery
  - json
url: 762.html
id: 762
categories:
  - jQuery
date: 2013-09-04 10:42:49
---

![G03A0003](/uploads/2009/01/g03a0003.jpg) A few months ago, I demonstrated [how to use ASP.NET's JSON capabilities](/2008/08/04/aspnet-ajax-using-json-heres-how/).  Lately, I've been demonstrating how to use jQuery.  Both use a considerable amount of JavaScript so if there is some way we could eliminate or reduce the amount of code we had to load, performance would naturally increase. Fortunately, there is.  **What stays the same** You are still going to create a JSON enabled web service just like you did in the earlier article by giving the web service class the attribute, "ScriptService."  You remember, this is what enables the web service to return JSON. **What is different** You will not, however, be including this block in your ASPX file:

      <asp:ScriptManager ID="ScriptManager1" runat="server">
        <Services>
          <asp:ServiceReference path="~/WebService.asmx" />
        </Services>
      </asp:ScriptManager> 

[](//11011.net/software/vspaste)You might still need the ScriptManager if you are using MS-AJAX on your page, but if you don't have any other MS-AJAX on your page, you can remove the entire block of code. In the jQuery for your page, your code will look something like this:

$.ajax({
    type: "POST",
    url: "WebService.asmx/Add",
    data: "{a:1,b:4}",
    contentType: "application/json; charset=utf-8",
    dataType: "json",
    success: function(result) {
        alert(result.d);
    }
});

You can get the full documentation for the $.ajax() global method here: [http://docs.jquery.com/Ajax/jQuery.ajax](//docs.jquery.com/Ajax/jQuery.ajax "http://docs.jquery.com/Ajax/jQuery.ajax") A few things that need to be pointed out here:

1.  For this to work, type must be "POST."
2.  url: is the name of the asmx file followed by a slash followed by the name of the web service method you want to call.
3.  data: is a JSON name/value pair list of all the parameters in the form of {parametername: parameterValue\[,...\]} if there are no parameter just use "{}".  Don't use "" or your code will not work.
4.  contentType and dataType tell jQuery we are working with JSON.
5.  The function pointed to by success will be called when the request has finished successfully, it will pass in a variable of type json.  The d property holds the return value.
6.  If the return value is a structure or class d will have properties hanging off of it specifying

1.  type information of the structure or class
2.  property for each property/variable in the structure or class.

You could take most of that AJAX call and turn it into some sort of helper function, but even if you don't the amount of code you will end up loading using this method is significantly less than what you would load using both together.
