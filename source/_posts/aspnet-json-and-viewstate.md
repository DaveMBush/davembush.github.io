---
title: ASP.NET JSON  and ViewState
tags:
  - asp.net
  - json
  - viewstate
url: 784.html
id: 784
categories:
  - ASP.NET
date: 2012-09-25 05:44:20
---

![image](/uploads/2009/01/image5.png)I received the following question recently about my article "[ASP.NET AJAX using JSON - Here’s how.](/2008/08/04/aspnet-ajax-using-json-heres-how/)"

> If we update the value of a textbox or label via a JSON web service call - will the value of that textbox/label be written to the viewstate or whatever so that the server side code can see the new values that came from the JSON request?

<!-- more -->

The short answer is, "no, it will not update viewstate."  But I think it would be helpful to understand when this is important rather than just giving you a blanket answer.

First, let's take a look at exactly how view state is handled in ASP.NET.

![image](/uploads/2009/01/image5.png)

## Normal Processing of an ASP.NET page

 When the browser first requests the ASPX page, the page is instantiated.  Since this is the first time it is being requested, there is no viewstate, so the deserialize step is skipped and the form elements step is skipped, and our page creation code is run.

 The magic happens right before the page is rendered and sent back to the client.  At this point, the state of all the `runat="server"` elements we have on the page are serialized (stored) into the viewstate object which typically is stored in the hidden field on the form.  There are providers, however, that allow you to store this same information on the server someplace.  Then the HTML for the page is rendered and sent back to the browser.

 The next request from the browser using the same page is a POST form request.  The first thing that happens after the page is instantiated and all  the `runat="server"` objects are instantiated, is that the viewstate is deserialized (retrieved from the viewstate object) so that by the time we get to the parsing of the form elements, all of our objects are back to exactly the same state that they were when we first sent the page back to the browser.

 Once the elements have been restored, the form elements are parsed and the appropriate properties on our form elements are set (Textbox.Text for example) and any events where we might need the change event fired are determined based on the difference between the view state and the current state of the form element.

 In the case of JSON calls, either using ASP.NET or jQuery, all we are updating is the element.  The question really becomes, "Does it matter?"

 ## Does it matter?

 By default, all elements on the page are serialized and deserialized into and out of viewstate.  But in the case of things like a Textbox, most of the time we don't care.  The fact of the matter is that most of our applications would work perfectly fine with viewstate turned off.

 Let's take a look at what happens with a text box.

 When the text box is initially rendered on the page, it may have the current content from the database.  The person at the browser may change the form element and then submit the page.  Assuming we don't need to fire the changed event on the server, the page is instantiated, the text box object is created, the text box object is restored to its original state and then immediately set to the content that was in the text box when the page was submitted.

 By the time we get to our event handling code, we don't care, nor do we know, what the content of the field was the last time the page was sent back to the browser.  The code would work just as well with the viewstate for that element turned off.  This is mostly true of all the elements on the screen and since not all elements use viewstate to determine what events get fired it is more a matter of experimenting by turning viewstate off and seeing if the code still works than any hard and fast rule that determines if we need viewstate turned on for the element.

 The case of a label is different.  Since it is not a form element, but is rendered as a div, it's content will not be sent back to the server with the post.  The only way of avoiding having to compute the value of the label is either by storing it in viewstate, or by storing it in viewstate or someplace on the server.

 ## So why is viewstate turned on for everything by default?

  Microsoft has a tendency to make the default behavior idiot proof.  They know that no one will read the documentation on how viewstate works or when it should be used.  So they set it up so that it will work for everyone out of the box.

  I've seen enough "wrong" code and heard enough complaints about how this new model works to know that had Microsoft set it up so that turning viewstate on was an option and not the default, most programmers would still be using ASP at this point and ASP.NET would have been a dismal failure... and for those of you who think it is, it would have been an even bigger disaster.

  In the case of using JSON with your ASP.NET code, your best bet would be to turn viewstate off for the page and assume that it isn't going to do anything for you.  The point of JSON is to make your page more lightweight.  Turning viewstate on adds extra baggage to your HTML that you probably won't need anyhow.
