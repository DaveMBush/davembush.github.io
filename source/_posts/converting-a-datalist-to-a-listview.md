---
title: Converting a DataList to a ListView
tags:
  - asp.net
  - datalist
  - listview
url: 109.html
id: 109
categories:
  - ASP.NET
date: 2013-12-25 20:15:26
---

Yesterday, I spent the bulk of the day converting a DataList to a ListView.  I thought I'd spend a little time relating the process for those of you who might be interested in doing the same.

For the most part, the transition went smoothly.

The first thing I looked at was the structure of the two sets of tags.  It was pretty quickly apparent that I was not going to be able to just change the tag name.  Not that I expected I could, but it would have been a nice bonus. 

What I did notice as I inspected the structure was that both sets have an ItemTemplate tag.  So I create a new ListView control pointing at the data I want it to display, and then I moved the itemTemplate tag from the DataList into the new ListView.

Bad move.  But, since I've already suffered the pain, it won't be so bad for you.  The basic problem stems from the fact that the ListView itemTemplate expects to start and end with a pair of TD tags.  In the DataList's itemTemplate tag, you can put anything you want.  So, what you should do is copy the contents of the itemTemplate tag in the DataList into the the ListView's itemTemplate tag and then put the TD tags around the content you just pasted, and inside the itemTemplate tags.

Not so bad once you figure it out, but if you start this process after a full day of programming, this can be just a bit frustrating.