---
title: Overriding Events in VB.NET
tags:
  - events
  - intellisense
  - overrides
  - vb.net
url: 153.html
id: 153
categories:
  - VB.NET
date: 2008-05-08 17:20:17
---

Back in the day, you use to be able to override an event in VB by using the drop down list in the code window.  Today, I had a friend who is moving from Visual Studio 2003 to Visual Studio 2008 ask me, "How do I override events in VB.NET now?"  All you get in the drop downs now is the event handlers. Well, I'll be honest here.  I had no idea that was even an option. It turns out that the best way to automatically override events using VB.NET now is to make use of the Intellisense everywhere feature. If you type in, "prot", you can select "protected" from the list.  Press the space bar and then type in "overr" and select "overrides" from the list.  One more space and the list of overridable functions appear.  Select one and press tab to get the function all stubbed out for you. It's not quite the same, but I think in the end it may actually be faster than using the drop down list boxes at the top of the edit screen.