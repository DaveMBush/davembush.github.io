---
title: Prevent Postback on Buttons
tags:
  - asp.net postback button
url: 1888.html
id: 1888
categories:
  - ASP.NET
date: 2010-10-11 09:04:04
---

![IMG_1380](/uploads/2010/10/IMG_1380.jpg "IMG_1380") Over the weekend I got a question about how to prevent postbacks on buttons from within jQuery tabs.  But the question really isn’t specific to jQuery.  There are other times when you might not want a button to post back.  So how do you do this? There are several ways you might accomplish this depending on what your goal is.  The first, and most obvious choice, is to not use an ASP:Button control and use an HTML input type=”button” tag instead.  This will allow you to have full control over what is happening on the client side.  If at all possible, this should be your first choice.

<!-- more -->

``` html
<input type="button" value="button name" />
```

You will need to attach your own JavaScript to this so that it does what you want. If that doesn’t work for you, then you can make use of the OnClientClick property of the ASP:Button control (this also works for other controls that post back)

``` html
<script language="javascript">
    function javascriptfunction() {
    // other code you want to execute on the client side
        return false;
    }
</script>
<asp:Button ID="Button1" runat="server" Text="Button"
    OnClientClick="javascriptFunction" />
```

The “return false;” in the javascript function will cause the control to not post back.
