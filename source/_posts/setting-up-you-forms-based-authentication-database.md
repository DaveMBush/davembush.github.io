---
title: Setting Up Your Forms Based Authentication Database
tags:
  - asp.net
  - aspnet_regsql
  - 'c#'
  - forms based authentication
url: 1416.html
id: 1416
categories:
  - forms authentication
date: 2009-09-14 05:41:24
---

![B01I0003](/uploads/2009/09/B01I0003.jpg "B01I0003")

I was recently asked if I would cover some topics related to Forms Based Authentication.  The person who requested this information has some specific issues that he wants covered that I won’t be covering for a while because I think there are some other issues that need to be covered first.

One of those is setting up the database.

<!-- more -->

When I owned my hosting companies, I saw more confusion in this area than just about any other topic that came up.

So here’s the step-by-step process you need to go through to set up your database for Forms Based Authentication.

In your hosting company’s control panel, create the SQL database.  You’ll want to create a database with a size of at least 10 meg with 5 meg allocated to the log file and 5 meg allocated to the database.  You’ll also want to make sure the user you attach to the database has DBO rights.  You may need to contact support to enable this.

Next, you’ll want to run aspnet_regsql.exe which you can find under c:\\windows\\Microsoft.NET\\Framework\\v2.0.50727.  This will provide you a wizard interface and will ultimately create the appropriate tables for you. The wizard is self explanatory, so I won’t repeat it here except to say that you’ll want to enter your connection information to connect to the server you just created your database on, not one of your databases locally.

Next you’ll need to add the connection information to your web.config file for the application you are setting up Forms Based Authentication for.

``` xml
<connectionStrings>
 <remove name="LocalSqlServer"/>
 <add name="LocalSqlServer"
  connectionString="Data Source=sqlserverGoesHere;
    Initial Catalog=YourDatabaseName;
    Persist Security Info=True;
    User ID=YourSqlUserID;
    password=YourSqlPassword"
  providerName="System.Data.SqlClient"/>
</connectionStrings>
```

The “remove” element is needed because typically the machine.config file on the server has its own entry that you won’t be using.

Unfortunately, you’ll need to create your own UI for adding users and assigning them to roles.  It isn’t that hard to do and once you’ve done it you can move the administration controls from project to project.  If I were you, I’d create the control set once in a special sub-directory so that I could move it from project to project.
