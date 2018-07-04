---
title: Storing a DataRow into a Session (server) variable
tags:
  - asp.net
  - datarow
  - datatable
  - session server
url: 73.html
id: 73
categories:
  - ASP.NET
date: 2008-01-15 18:42:47
---

I recently ran into a situation where I needed to store a DataRow object, which is not serializable, into a Session variable using the Session Server.  As I mentioned yesterday, all sessions should be stored to either the Session Server or the SQL Session Server.  This means that all of the objects being stored must be serializable. My problem stemmed from the fact that a DataRow is not Serializable and there is no clean mechanism for it to become serializable. However, a DataTable is serializable.  So, the simple solution is to:

*   Create a DataTable object
*   Add the DataRow to the Table
*   Store the DataTable in the Session variable.

To get the row out, you reverse the process.

*   Retrieve the DataTable from the Session object
*   Cast the object to a DataTable
*   Retrieve the first row from the table.