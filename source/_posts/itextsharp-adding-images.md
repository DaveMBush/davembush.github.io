---
title: iTextSharp – Adding Images
tags:
  - 'c#'
  - images
  - iTextSharp
  - PDF
url: 1212.html
id: 1212
categories:
  - iTextSharp
date: 2009-06-30 07:33:00
---

![Maple leaves in Autumn.](/uploads/2009/06/AutumnLeaves.jpg "Maple leaves in Autumn.")

Last week I showed how to use form fields to control placement of dynamic data.

But what if you want to dynamically place images in your PDF?  You can stuff them into a form field like you can with text.

<!-- more -->

However, one of the items you can retrieve from the form field is its location on the screen.  Using this, a little math, and some iTextSharp image code, we can place images in our PDF where the form field was located.  Here’s how I do it.

First, this code builds on the previous code I’ve already demonstrated in previous articles.  If this is your first time here, you’ll want to scroll to the bottom of this post where it says, “Other Posts in iTextSharp” and read them first.

To retrieve the position of the form field, you’ll need to call the field’s GetFieldPositions() method.  It is plural because it is going to get the position of every field in the document with the name you specify.  For our purposes, we will assume that the field only exists once and is on the first page.

``` csharp
float[] topImagePosition = null;
topImagePosition =
    fields.GetFieldPositions("m_topPicture");
```

The float array that is returned has five elements:

*   Page Number (starting at 1)
*   Left
*   Bottom
*   Right
*   Top

If the page rotation is 90 degrees, you’ll want to rotate the position information.  You can check the rotation using

``` csharp
rotation = pdfReader.GetPageRotation(1);
```

Where `GetPageRotation()` takes a parameter representing the page number, starting at 1.

You can rotate the positions using this code.

``` csharp
left = topImagePosition[1];
right = topImagePosition[3];
top = topImagePosition[4];
bottom = topImagePosition[2];
if (rotation == 90)
{
    left = topImagePosition[2];
    right = topImagePosition[4];
    top = pageSize.Right - topImagePosition[1];
    bottom = pageSize.Right - topImagePosition[3];
}
```

Next, you’ll want to retrieve the image you want to display on the PDF.  There are APIs for retrieving the image from a web site, but most of the time you’ll be retrieving the image from your hard drive.  Here is the code to do that.

``` csharp
iTextSharp.text.Image topImage =
    iTextSharp.text.Image.GetInstance(
    Server.MapPath(imagelocation +
    productRow.imagefilename));
```

Before we place the image on the page, the next thing you’ll want to do is scale the image so that it fits in the rectangle

``` csharp
topImage.ScaleToFit(right - left, top - bottom);
```

And then we position it

``` csharp
topImage.SetAbsolutePosition(
    left + ((right - left) - topImage.ScaledWidth) /
    2, top - topImage.ScaledHeight);
```

and then we put it into the PDF

``` csharp
PdfContentByte contentByte = stamp.GetOverContent(1);
contentByte.AddImage(topImage);
```

Notice the contextByte thing.  That’s the critical part of how we get the images into the PDF.

Finally, you’ll want to remove the field from the form so that it doesn’t show up.

``` csharp
fields.RemoveField("m_topPicture");
```

I’m not sure this is really needed, but it doesn’t hurt to clean up things you no longer need.
