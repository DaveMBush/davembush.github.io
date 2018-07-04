---
title: CSharp IDisposable Confusion
tags:
  - 'c#'
  - idisposable
url: 2579.html
id: 2579
categories:
  - 'c#'
date: 2014-07-17 13:00:00
---

I’m planning to get my MCSD certification next and the first exam I plan to take is the 70-483 which will test my knowledge of CSharp. To study, I got this free PDF: [MCSD Certification Toolkit (Exam 70-483)](//www.it-ebooks.info/book/2564/) (Which I wouldn’t recommend, but I haven’t found anything yet that I WOULD recommend, so this will have to do.) In this book (Chapter 5) and other places on the web, it makes this statement:

*   If a class contains no managed resources and no unmanaged resources, it doesn’t need to implement IDisposable or have a destructor.
*   If the class has only managed resources, it should implement IDisposable but it doesn’t need a destructor. (When the destructor executes, you can’t be sure managed objects still exist, so you can’t call their Dispose methods anyway.)
*   If the class has only unmanaged resources, it needs to implement IDisposable and needs a destructor in case the program doesn’t call Dispose.
*   The Dispose method must be safe to run more than once.  You can achieve that by using a variable to keep track of whether it has been run before.
*   The Dispose method should free both managed and unmanaged resources.
*   The destructor should free only unmanaged resources.  (When the destructor executes, you can’t be sure managed objects still exist, so you can’t call their Dispose methods anyway.)
*   After freeing resources, the destructor should call GC.SuppressFinalize, so the object can skip the finalization queue.

Managed VS Unmanaged?
---------------------

So what is the difference between a managed resource and an unmanaged resource?  An unmanaged resource is something that is not under the direct control of the .NET memory manager.  So, file handles, connections to the database, memory handles, and other OS items fall under the realm of “unmanaged.”  Managed code is everything else. So, if you read the points above and read the statements about managed resources, you would think, or should think, “That’s already under the control of the .NET memory manager, so I shouldn’t have to do ANYTHING!” But with one little tweak, this all becomes clear.  What this block of text is talking about specifically is managed code that is referencing other code that implements IDisposable.

Corrected Version
-----------------

So, let’s rephrase the block of text to the following:

*   If a class contains no managed resources that implement IDisposable and no unmanaged resources, it doesn’t need to implement IDisposable or have a destructor.
*   If the class has only managed resources that reference resources that implement IDisposable, it should implement IDisposable but it doesn’t need a destructor. (When the destructor executes, you can’t be sure managed objects still exist, so you can’t call their Dispose methods anyway.)
*   If the class has only unmanaged resources, it needs to implement IDisposable and needs a destructor in case the program doesn’t call Dispose.
*   The Dispose method must be safe to run more than once.  You can achieve that by using a variable to keep track of whether it has been run before.
*   The Dispose method should free both managed resources that implement IDisposable and unmanaged resources.
*   The destructor should free only unmanaged resources.  (When the destructor executes, you can’t be sure managed objects still exist, so you can’t call their Dispose methods anyway.)
*   After freeing resources, the destructor should call GC.SuppressFinalize, so the object can skip the finalization queue.

One Other Circumstance
----------------------

There is one other situation where you might want to implement IDisposable on a class that doesn’t reference an object that implements IDisposable.  While this particular case is rare, I think it is probably good to list it here for completeness. If you have a class that will consume a lot of memory either directly or indirectly, you might want to consider implementing IDisposable and the Dispose method so that any class that is calling this method has a way of immediately releasing the memory the class is using by dereferencing the memory it is using, and then calling:

    GC.Collect();
    GC.WaitForPendingFinalizers();
    GC.Collect();

I can already hear some of you saying, “but you shouldn’t have to ever need to do this!” And yes, 99% of the time, you shouldn’t.  But, if you have an issue with this, talk to [the guys over at the Microsoft Virtual Academy](//www.microsoftvirtualacademy.com/training-courses/developer-training-with-programming-in-c) where I learned this.

#### Other Places Talking About IDisposable and .NET Memory Management

*   [IDisposable and Using in C#](//coding.abel.nu/2011/12/idisposable-and-using-in-c/)
*   [IDisposable, Finalizer, and SuppressFinalize in C# and C++](//manski.net/2012/01/idisposable-finalizer-and-suppressfinalize/)
*   [Common Pitfalls with IDisposable and Using](//manski.net/2012/01/idisposable-finalizer-and-suppressfinalize/)
*   [CSharp Memory Management](//www.technofranchise.com/c-memory-management/)
*   [Disposal and Garbage Collection in CSharp](//www.go4expert.com/articles/disposal-garbage-collection-c-sharp-t30059/)