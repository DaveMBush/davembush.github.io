---
title: Tab Control ActiveTabIndex Lost on Postback
tags:
  - activetabindex
  - ajax
  - postback
  - tab control
  - tabcontrol
url: 850.html
id: 850
categories:
  - ASP.NET
date: 2009-02-25 05:26:36
---

![tran-land-01](/uploads/2009/02/tran-land-01.jpg) I just got off the phone with a client who is using the MS-AJAX TabControl in one of his applications and any time he causes a postback, the tab resets to the first tab. If you've never seen the problem, you're lucky.  There are a couple of ways around the problem.  The first and easiest if it works in your situation is to put the tab in an update panel so that you never actually do a full postback. However, there are times when this won't work.  In this particular case it is because one of the tabs holds the file upload control, which can't be used inside an update panel.  (Another problem we had to find a way around last week.) What do you do then? Turns out this is a bug in the way that the tab works.  This problem turns up in various situations.  If you are fighting the bug, the solution is actually pretty simple. First, put the following  javascript in your aspx or ascx file:

    <script type="text/javascript">
        function TabChanged(sender, args)
        {
            sender.get_clientStateField().value =
                sender.saveClientState();
        }
    </script> 

And then wire up the TabChanged function to your tab control:

<ajaxControlToolkit:TabContainer ID="TabContainer1" runat="server" OnClientActiveTabChanged="TabChanged" >
</ajaxControlToolkit:TabContainer\> />

  This will fix the problem.
