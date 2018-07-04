---
title: Computed Columns Using LINQ to SQL Classes
tags:
  - 'c#'
  - linq
  - sql
  - video
  - visual studio
url: 100.html
id: 100
categories:
  - 'c#'
date: 2008-02-12 08:16:14
---

Last week we looked at the extension points Microsoft has wired into the LINQ to SQL classes and how they can be used to achieve some of the capabilities of the Business Logic Layer (BLL) in a multi-layered architecture. We saw that this feature allowed us to modify data as it was coming out of the database up to the presentation layer and from the presentation layer down to the database.  But, what about the times when what you really want is another field in your row that is based on the other fields in your database? You actually have two choices.  You could create a SQL statement that computes the columns for you.  Or you can add a property to your LINQ to SQL classes that computes the value for you.