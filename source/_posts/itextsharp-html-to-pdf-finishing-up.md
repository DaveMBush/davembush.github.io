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

 1: isBlock = Resources.html2pdf

 2:    .ResourceManager

 3:    .GetString(tagName + "_isBlock");

 4: if (isBlock != null && 

 5:    isBlock.ToLower() == "true")

 6: {

 7:    isBlock = Resources.html2pdf

 8:        .ResourceManager

 9:        .GetString(tagName + "_crlfAtEnd");

 10:     if (isBlock != null && 

 11:        isBlock.ToLower() == "true")

 12:    {

 13:        et = stack.Peek();

 14:        Font f = getCurrentFont();

 15:        if (et is Phrase)

 16:        {

 17:            ((Phrase)(et)).Add(

 18:                new Chunk("\\n\
", f)); 

 19:            stack.Pop();

 20:        }

 21:    }

 22:    p = new Paragraph();

 23:    ((Paragraph)p).Add("");

 24:    ((Paragraph)p).SetLeading(m_leading, 1);

 25:    list.Add(p);

 26:    stack.Push(p);

 27: }

One problem I’ve had with this in the past is that this cr/lf gets added at the end even if the block is the last block.  I really need to find some way to detect that this is the last place this occurs either nested or in the outermost block.  But I’ll leave that enhancement for you.

.csharpcode, .csharpcode pre { font-size: small; color: black; font-family: consolas, "Courier New", courier, monospace; background-color: #ffffff; /\*white-space: pre;\*/ } .csharpcode pre { margin: 0em; border: 0px;padding:0px; } .csharpcode .rem { color: #008000; } .csharpcode .kwrd { color: #0000ff; } .csharpcode .str { color: #006080; } .csharpcode .op { color: #0000c0; } .csharpcode .preproc { color: #cc6633; } .csharpcode .asp { background-color: #ffff00; } .csharpcode .html { color: #800000; } .csharpcode .attr { color: #ff0000; } .csharpcode .alt { background-color: #f4f4f4; width: 100%; margin: 0em; } .csharpcode .lnum { color: #606060; }
