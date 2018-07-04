---
title: iTextSharp – HTML to PDF - Prerequisites
tags:
  - asp.net
  - 'c#'
  - html
  - iTextSharp
  - PDF
url: 1284.html
id: 1284
categories:
  - iTextSharp
date: 2009-07-14 06:40:39
---

![animal-015](/uploads/2009/07/animal015.jpg "animal-015")

Before we get into the nitty gritty of parsing the HTML so that we can create PDF code from it, it is important that we develop the concept of how text layout works in iTextSharp.  So today we will cover those basics.

The first type of element we want to deal with when we parse our HTML into a PDF is the Paragraph element.

When we get to actually parsing our HTML to PDF code we will use the Paragraph object for all of our block elements.  This allows us to add other Paragraphs and Chunks into it which we can format.

A Chunk is our second object that we will be using.  The Chunk is the main object that will allow us to format the font.  In fact, even if our block element specifies some sort of specific font, the font doesn’t actually get applied in the code until we add the text.

Typical code to place text into a PDF document would look something like this

p = new Paragraph(new Chunk("text that needs a font", 
    FontFactory.GetFont("Arial", 10, Font.NORMAL, Color.BLACK)));
p.Alignment = (Element.ALIGN_CENTER);
ct.AddElement(p);

[](//11011.net/software/vspaste)where “ct” is an object of type ColumnText that we discussed last week.

The only other two classes we need to discuss are the list classes.  We use the List to create an item that will handle both the OL and UL tags.  The ListItem class will handle the individual items within the list.  The List constructor handles which of the two types of list we are dealing with by specifying true or false in the first parameter, numbered.

I have not yet added the ability to handle tables to my HTML parser mainly because I have not had the need.  I think once I show you how to create tables and how to parse HTML you should be able to handle adding table parsing code yourself.
