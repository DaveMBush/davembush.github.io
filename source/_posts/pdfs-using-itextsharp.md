---
title: PDFs Using iTextSharp
tags:
  - asp.net
  - iText
  - iTextSharp
  - PDF
url: 1190.html
id: 1190
categories:
  - iTextSharp
date: 2009-06-17 08:06:00
---

![iStock_000002747386Medium](/uploads/2009/06/iStock_000002747386Medium.jpg "iStock_000002747386Medium") There are several libraries on the market now that allow you to create PDF documents from your .NET applications.  The one I’ve chosen to use is [extSharp](//sourceforge.net/projects/itextsharp/), an open source library that is a port of the open source library for Java,  [iText](//www.lowagie.com/iText/).

While there are several sites on the Internet that provide examples of how to use iText, I’ve found that the documentation for iTextSharp is a little harder to come by.  So I thought it might be helpful if I provided some posts on how I use iTextSharp along with some of the gotchas I’ve encountered along the way.

-more-->

To use iTextSharp, you will need to add a reference to the library in your code, or simply drop the code into your bin directory of your ASP.NET application.

The main trick in translating the iText samples and documentation to iTextSharp is that iTextSharp makes the Java properties in iText (get_PropertyName_()/set_PropertyName_()) .NET properties, (PropertyName).

You’ll also need to know that the namespace for iTextSharp is different from iText.  Be sure to include the following line at the top of any code you write that uses iTextSharp

using iTextSharp.text;
using iTextSharp.text.pdf;

[](//11011.net/software/vspaste)

To get your ASP.NET page to return a PDF file, you’ll want to add the following code at the top of your Page_Load event handler:

Response.Clear();
Response.ContentType = "application/pdf";
Response.AddHeader("ContentType", "application/pdf");
Response.AddHeader("Content-Disposition", 
    "inline;filename=\\"FileName.pdf\\"");

[](//11011.net/software/vspaste)The Response.Clear() line clears out any input that has already been sent back to the browser.

The next line tells the browser that what is coming back is a PDF, not HTML.

The “Content-Disposition” line allows the browser to know that we want the file IN the browser window rather than downloading the file.

Finally, yes, I know there are two ContentType lines.  But I took this from working code and while I can’t remember why I wrote it like that, it works.  If you can get it working with only one line, go for it.

Now that we have the basics out of the way, we can concentrate on actually generating PDFs in future posts.
