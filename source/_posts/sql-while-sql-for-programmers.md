---
title: SQL WHILE - SQL For Programmers
tags:
  - do while
  - for next
  - ms-sql
  - sql
  - tsql
  - while
url: 443.html
id: 443
categories:
  - SQL For Programmers
date: 2014-02-12 23:41:49
---

The IF statement we looked at on Tuesday was pretty tame compared to the WHILE construct. Actually, the main thing you need to keep in mind is that WHILE is all you have.  There is no such thing as a FOR loop or a DO WHILE loop.  So, you have to force WHILE to do those for you. The basic syntax of WHILE looks like this:

<!-- more -->

``` sql
DECLARE @someString as VARCHAR
WHILE @someString='ABC' BEGIN
   SELECT * FROM someTABLE
   SELECT * FROM someOtherTABLE
  END
```

So if you want a FOR/NEXT loop, you'll need to write:

``` sql
DECLARE @someInt as int
SET @someInt = 0
WHILE @someInt < 20
  BEGIN /* useful code here */ SET @someInt = @someInt + 1
  END
```

and a DO WHILE loop would be something like:

``` sql
DECLARE @someInt as int
SET @someInt = 0
WHILE @someInt = 0
  BEGIN /* useful code here */ IF /*some exit condition */ SET @someInt = 1
  END
```

Once you learn to substitute those constructs for your normal FOR/NET or DO WHILE code, it becomes rather easy to deal with. Now if they'd just replace BEGIN with { and END with } I think I could live with this.
