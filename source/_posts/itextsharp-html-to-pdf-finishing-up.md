---
title: iTextSharp – HTML to PDF – Finishing Up
tags:
  - .net
  - 'c#'
  - html
  - iTextSharp
  - PDF
url: 1372.html
id: 1372
categories:
  - iTextSharp
date: 2009-08-12 06:19:24
---

![tiger](/uploads/2009/08/tiger.jpg "tiger") In the last post I mentioned there were a few topics we need to close up today.  The two topics we’ve left undone are popping the attribute information off the stack when we hit a closing element and dealing with the paragraph gap that normally appears between paragraph elements.

The first thing you’ll want to do when you hit a closing element is to retrieve its name again.  Just like we did at the beginning element.  Once you have that you can pop the attribute information off the stack(s).

You’ll also want to undo any indentation that you applied during the opening element.

To handle the paragraph break, I defined a _crlfAtEnd attribute in my resource file.  If it was defined as true, I added an extra line feed at the end to account for the gap.

``` csharp
isBlock = Resources.html2pdf
  .ResourceManager
  .GetString(tagName + "_isBlock");
if (isBlock != null &&
  isBlock.ToLower() == "true")
{
  isBlock = Resources.html2pdf
    .ResourceManager
    .GetString(tagName + "_crlfAtEnd");
  if (isBlock != null &&
    isBlock.ToLower() == "true")
  {
    et = stack.Peek();
    Font f = getCurrentFont();
    if (et is Phrase)
    {
      ((Phrase)(et)).Add(
        new Chunk("\n", f));
      stack.Pop();
    }
  }
  p = new Paragraph();
  ((Paragraph)p).Add("");
  ((Paragraph)p).SetLeading(m_leading, 1);
  list.Add(p);
  stack.Push(p);
}
```

One problem I’ve had with this in the past is that this cr/lf gets added at the end even if the block is the last block.  I really need to find some way to detect that this is the last place this occurs either nested or in the outermost block.  But I’ll leave that enhancement for you.
