---
title: GridView and Updating A Row Manually
tags:
  - asp.net
  - 'c#'
  - gridview
  - gridviewupdateventargs.cancel
  - rowupdating
  - update
url: 1507.html
id: 1507
categories:
  - ASP.NET
date: 2009-11-04 07:36:08
---

![G07L0095](/uploads/2009/11/G07L0095.jpg "G07L0095") A couple of days ago I mentioned a project that I’ve been working on that is a bit out of the ordinary as far as GridViews go.  One of the issues I’ve had is that the edit template doesn’t map to the view template very well. OK, it doesn’t map at all. You see, the data that gets stored back to the database during the edit could go to two of four different tables.  But the view is generated from a stored procedure that gathers the information from those tables and makes the result look like it came from one table. So how do you have the GridView update the database under these conditions?  The secret lies in the event handlers--in this case, the RowUpdating event handler. Using the RowUpdating handler, we can do whatever we want.  The first line of code you need to make sure you include is

e.Cancel = true;

“e” represents the System.Web.UI.WebControls.GridViewUpdateEventArgs object that is passed in.  What the line of code says is “cancel any other update processing that may have happened after this event gets called.” From here on out, you’ll need to retrieve the values from your edit controls manually  using the [FindControByID() method I demonstrated on Monday](/2009/11/02/access-a-control-by-id-from-within-a-databound-control/) and updating the database using code.   **Other places talking about the GridView and Updating a Row** [sitecore with gridview – rowupdating event doesn't fire](//blog.paulgeorge.co.uk/2009/03/25/sitecore-with-gridview-rowupdating-event-doesnt-fire/) \- gridview to in web.config resolves this issue. your web.config would then look like : system.web.ui.webcontrols.repeater system.web.ui.webcontrols. ... [DataBinding in ASP.NET 2.0 and the RowUpdating event - Dennis van ...](//bloggingabout.net/blogs/dennis/archive/2006/08/24/DataBinding-in-ASP.NET-2.0-and-the-RowUpdating-event.aspx) \- For a while now I'm trying to figure out why my method, triggered by the GridView.RowUpdating event, doesn't work as all samples say it should do. All samples of course assume you're doing everything in your .aspx page, but I have to do ... [Simple Insert, Select, Edit, Update and Delete in Asp.Net GridView ...](//www.aspdotnetcodes.com/GridView_Insert_Edit_Update_Delete.aspx) \- The above block of codes in RowUpdating event, finds the control in the GridView, takes those values in pass it to the CustomersCls class Update method. The first parameter GridView1.DataKeys\[e.RowIndex\].Values\[0\]. ...
