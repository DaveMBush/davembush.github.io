---
title: iTextSharp – HTML to PDF – Positioning Text
tags:
  - asp.net
  - 'c#'
  - iTextSharp
  - PDF
url: 1260.html
id: 1260
categories:
  - iTextSharp
date: 2009-07-08 06:50:15
---

![](/uploads/2009/07/misc_vol1_012.jpg)

The next series of things I’m going to introduce about using iTextSharp are all going to lead toward taking HTML text and placing it on the PDF document.

There are several items we need to cover before we even get to the part about converting the text from HTML to PDF text.  The first is placing the text on the document where it is supposed to be.

_Once again, we are building on previous articles about using iTextSharp.  So if you are just jumping in, you might want to go take a look at the other articles.  You can find a list at the bottom of this post._

To place a block of text on the screen that is going to have multiple formats in it (bold, underline, etc) I use the ColumnText class.  This allows me to specify the rectangle or, if I want, some irregular shape, to place the text in.  I handle determining where this rectangle is on the page in the same way that I determine where an image should go.  I have the designer place a form field on the screen and then I use that to get my coordinates.

float\[\] fieldPosition = null;
fieldPosition = 
    fields.GetFieldPositions("fieldNameInThePDF");
left = fieldPosition\[1\];
right = fieldPosition\[3\];
top = fieldPosition\[4\];
bottom = fieldPosition\[2\];
if (rotation == 90)
{
    left = fieldPosition\[2\];
    right = fieldPosition\[4\];
    top = pageSize.Right - fieldPosition\[1\];
    bottom = pageSize.Right - fieldPosition\[3\];
}

Once I have the position, the next thing I need to do is to create my ColumnText object.  This requires the same ContentByte object that we used for the images.

PdfContentByte over = stamp.GetOverContent(1);
ColumnText ct = new ColumnText(over);

And now I can set the rectangle to print into.

ct.SetSimpleColumn(left, bottom, right, top,
    15, Element.ALIGN_LEFT);

[](//11011.net/software/vspaste)The 15 represents the leading you want (space between characters vertically). You may need to adjust that number.

Once you have your rectangle, you can add paragraphs to it.  Paragraphs are composed of smaller units called chunks that can be formatted.  If you want a paragraph that is all formatted the same you can make a call that looks like this.

Paragraph p = new Paragraph(
    new Chunk("Some Text here", 
        FontFactory.GetFont(
          "Arial", 14, Font.BOLD, Color.RED)));

[](//11011.net/software/vspaste)and then add the paragraph to your rectangle

ct.AddElement(p);
