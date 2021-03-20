---
title: iTextSharp Tables
tags:
  - iTextSharp
  - PDF
  - tables
url: 1424.html
id: 1424
categories:
  - iTextSharp
date: 2009-09-17 06:43:14
---

![food-ml-04](/uploads/2009/09/foodml04.jpg "food-ml-04") PDF Tables in iTextSharp work enough like HTML tables that the slight differences between the two make programming tables for a PDF a bit confusing the first time you try.

I hope to describe some of those differences here so that your experience might be a bit smoother than mine was as you start to use tables in your PDF code.

One of the first decisions you’ll need to make as you start working on your iTextSharp table is how many columns the table should have.  Keep in mind that the cells can be spanned, just like they can in HTML, so it is better to have too many columns than the bare minimum.  You can always adjust later.

Once you’ve decided how many columns, you’ll need to decide how big you want each column to be.  Unlike HTML, the columns will not stretch if they are too small.

Here is your initial code:

<!-- more -->

``` csharp
PdfPTable table= new PdfPTable(5);
float[] colWidths = {70, 70, 200, 70, 70};
table.SetWidths(colWidths);
```

Keep in mind that the column widths are more of a weighting than an absolute unit. You’ll also need to determine how wide the iTextSharp table should be.  So far I’ve only needed to set this to 100%.

``` csharp
table.WidthPercentage = (100.0f);
```

Next you’ll want to set the default cell information.  This took me a while to decipher, and I’m still not exactly sure I completely understand it.  But here is an example from some code  I’ve created that sets the borders to white and sets the padding to 10 points.

``` csharp
table.DefaultCell.Border = (PdfPCell.BOTTOM_BORDER);
table.DefaultCell.BorderColor = New iTextSharp.text.Color(255, 255, 255);
table.DefaultCell.BorderColorBottom = New iTextSharp.text.Color(255, 255, 255);
table.DefaultCell.Padding = 10;
```

Next it is just a matter of adding the cells in left to right, top to bottom order.  While there is a way to specify the row and column you want to place the content into using another API, the default API expects the information in order.

You can do this using the AddCell() method.  Again, this method will take a string, so you can add the content directly.  But if you are going to need control over the font size, I recommend that you first create a PdfPCell() object that has the content, passing its constructor a Phrase() object.  This will allow much more control.

``` csharp
table.AddCell(New PdfPCell(New Phrase("Last Name")));
```

If you need to you can adjust the fonts for an entire row using a loop.

``` csharp
foreach(PdfPCell c in (PdfPRow)(table.Rows(1)).Cells)
{
    c.Border = PdfPCell.BOTTOM_BORDER;
    c.BorderWidthBottom = 2;
    c.PaddingBottom = 4;
    c.PaddingTop = 2;
    (Chunk)(c.Phrase(0)).Font = headerFont;
}
```

Finally, it might be useful to include a header row and a footer row.  This way, as your table spans pages, the header and footer rows print on each page. The code for this is really rather simple, even though it wasn’t clear the first time I looked. You place your header row as the first rows in your table and your footer row right after the header rows, prior to the real content.  Then you specify your header rows in your code as.

``` csharp
table.HeaderRows = 2;
table.FooterRows = 1;
```

Which would make the first two rows a header that displays on each page and the third row a footer that displays on each page.

If you need to place your table at a specific location, you can use the column API to include the table in a column.
