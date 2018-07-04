---
title: GridViews - Multiple Rows Per Record
tags:
  - asp.net
  - gridview
  - multiple rows
  - templated columns
url: 676.html
id: 676
categories:
  - ASP.NET
date: 2008-12-17 07:57:14
---

![G04B0069](/uploads/2008/12/g04b0069.jpg) I don't know about you, but I've had several occasions where I've needed to use the simplicity of the GridView control in ASP.NET.  DataBinding, Paging, Sorting, etc.  But I've also wanted to stack the information for a record in multiple rows. Most of the time this happens when I need to display data that would take up two or three times as much of the width as my layout will allow. The solution to this problem is quite simple: Use templated columns and Literal controls.  Assuming you are starting off with a GridView control that renders like this:

Header1

Header2

Header3

Header4

Header5

Data1

Data2

Data3

Data4

Data5

etc...

But we want to display the data as:

Header1

Header2

Header3

Header4

Header5

Data1

Data2

Data3

Data4

Data5

etc...

To get your GridView to display like this, you will need to:

1.  Turn Header3 and Header4 into a templated columns.
2.  Add the following code into the TemplatedField section for column 3
    
    <HeaderTemplate>
       <asp:Label ID="Label3" runat="server" Text="Header3"></asp:Label>
       <asp:Literal ID="Literal1" runat="server" Text="&lt;/td&gt;&lt;/tr&gt;&lt;tr&gt;&lt;td colspan=&quot;2&quot;&gt;"></asp:Literal>
       <asp:Label ID="Label4" runat="server" Text="Header4"></asp:Label>
    </HeaderTemplate> 
    
3.  You'll want to modify the ItemTemplate and EditItemTemplate so that they have similar Literal tags and add the appropriate control after it for what WAS in column 4.

That's all there is to it.  Admittedly, there is a bit more work here than just drag and drop, but it beats having to write all of the databinding, paging and sorting code by hand.
