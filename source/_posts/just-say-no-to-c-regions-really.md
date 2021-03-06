---
title: 'Just say “No!” to C# Regions?  Really?!'
tags:
  - 'c#'
  - regions
url: 1021.html
id: 1021
categories:
  - 'c#'
date: 2009-04-16 06:31:49
---

![other-042](/uploads/2009/04/other042.jpg "other-042")  I just read a post by Casademora on “public abstract string\[\]  Blog()”

[Just say No! to C# Regions « public abstract string\[\] Blog()](//extractmethod.wordpress.com/2008/02/29/just-say-no-to-c-regions/)

and

[I still say Regions are not useful… but…](//extractmethod.wordpress.com/2008/06/02/i-still-say-regions-are-not-usefulbut/)

Arguing that not only should we NOT use code regions, but if we do, we are hiding “bad code.”  He uses words like “retarded,” “lame excuse for a preprocessor tag,” etc.

<!-- more -->

You’d think maybe this guy (gal?) just had a bad day when he wrote this, but no, he goes on to further defend his position in a later post.

And what I don’t understand at all is how his original post ended up on DotNetKicks today.  It’s an old post that was written last year but was just kicked up to the front page?  I figure someone MUST agree with him or it wouldn’t be there.

So, are Regions really as evil as Casademora argues that they are?

**First what is a region?**

``` csharp
#region Member Variables

private bool _refreshState;
private bool _isRefresh;

#endregion Member Variables
```

That’s a region.  See the #region and the #endregion preprocessor tags?  They define a named region in your code.

**Why do they exist?**

Any code you surround with a region is collapsible, just like your methods and properties are.  If you collapse the region above, all you see in your editor is:

![image](/uploads/2009/04/image1.png "image")

**Are they hiding “bad” code?**

The main argument that Casademora makes is that the use of regions hides “bad” code.  He defines bad code as:

Oh… wait!  He never defines “Bad” he only defines “Better.”

But I doubt that anyone could argue that declaration of private member variables is “bad” in any sense of the word.  In fact this code is from one of the best programmers I know.  Here’s a screen grab of the methods in this file:

![image](/uploads/2009/04/image2.png "image")

And the code in those methods is just as pretty.  In fact, Mike’s code is consistently well structured, well organized, follows good naming conventions.  I wish my code looked as good.

But when you first open the file, this is what you see:

![image](/uploads/2009/04/image3.png "image")

I don’t know about you, but I always feel like I’ve been hit with a breath of fresh air when I open Mike’s code.  Instead of being assaulted with 201 lines of code (that’s how many lines are in this file) I see this.

So the question is, would Mike’s code be as organized without the regions?  So I asked him.

He says, “I'd try to do it anyway, but the regions make it easier.  Like, if I need to add a new private method, I go right to that region.  I find it most useful to separate out the properties and event handlers.”

So, contrary to Casademora’s statement, regions actually help Mike structure his code so that it is easy to read.  They aren’t hiding “bad” code.  If anything they are “hiding” good code.

**Hiding Code is Good**

When you are working on code with lots of properties, methods, and event handlers.  Do you really need to see the member variables at the same time you are working on an Event handler?  Most of the time we don’t.

Regions, used correctly, make your code more readable, and hide code you aren’t working on so that you can easily find and work on the code you do need to work on.  That’s the whole point.

I’m sure one could find uses of regions that have nothing to do with code structure.  Uses that obfuscate the code rather than making it easier to maintain.  But if you use the code in a logical way, they can significantly increase the readability of your code.
