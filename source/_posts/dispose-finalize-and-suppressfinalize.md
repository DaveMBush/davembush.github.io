---
title: 'Dispose, Finalize and SuppressFinalize'
tags:
  - 'c#'
  - dispose
  - finalize
  - idisposable
  - suppressfinalize
url: 1262.html
id: 1262
categories:
  - 'c#'
date: 2013-06-12 04:04:38
---

![food-drk-017](/uploads/2009/07/fooddrk017.jpg "food-drk-017") I got the following question recently.

> What is the difference between Dispose and SupressFinalize in garbage collection?”

The problem with this question is it assumes Dispose and SupressFinalize have similarities, which I’m sure is not what is being asked here.  So let’s rephrase it in terms that make sense.

> I see three methods available to me in .NET that all seem to have something to do with garbage collection.  Can you explain what Dispose, Finalize, and SupressFinalize do and why I could use or call each one in my code?”

<!-- more -->

Now, that’s something we can answer.

## A Brief History of Memory Management

Before we answer the question, we need to review how memory management works in .NET compared to historical languages such as C/C++.

In the old days, if an application allocated memory on the heap (generally done with the “new” keyword in CSharp) the application was also responsible for releasing the memory.  In languages such as C++, this was done with the “delete” keyword which triggered a call to a special method called a destructor.  If the object was holding on to memory or other resources, it was the destructor’s job to release those resources.

In .NET memory is deallocated for us using garbage collection.  We no longer call the destructor using anything like a delete keyword and the deallocation process happens at some point in time that is after the memory goes out of scope.  This might be immediately after the object goes out of scope or it could be hours or days.

Generally, this is not a problem, but what about an object that is holding on to additional resources that need to be freed up right away? For that we have the Dispose() method.

## Dispose is the new destructor

While we don’t need a destructor to handle our memory, we do need something to release resources.

This is why the IDisposable interface was created with one method that needs to be implemented called Dispose().

If your class allocates resources that need to be freed up that aren’t memory, you should implement the IDisposable interface on that class and implement the Dispose() method.  Inside that method, you would release any resources that the class may have created and not freed.

## Dispose() will then automatically get called by Finalize()

When the garbage collector starts releasing memory, one of the things it will do is that it will go through your code and call Finalize() on each of the objects.  Finalize() will then call any dispose methods that are available.

This is done to protect the programmer from himself, not because it is expected.  Remember I said the memory will get freed up at some indeterminate point in the future but the Dispose() method was created so we could free up the resources right away? What we really want to do is call Dispose() ourselves.  We call Dispose from our own code.  Finalize() only exists so that Dispose() will get called if we don’t.

## And that’s where SupressFinalize() fits in.

If my code calls Dispose() then there is no reason for the Finalizer to run at all.  Without SupressFinalize() we would need to set a flag in our class indicating that Dispose() had already been called so that when Finalize() called it, we didn’t re-run the clean up code.

With SupressFinalize() we can just call SuppressFinalize() from within Dispose() and that will tell .NET to not run the Finalize method when it gets to our object.

Your call to SupressFinalize() should almost always look like

``` csharp
GC.SuppressFinalize(this);
```

but you can pass in any object instead of “this.”
