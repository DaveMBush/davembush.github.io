---
title: iTextSharp – The easy way
tags:
  - acrofields
  - form fields
  - iTextSharp
  - outputstream
  - PDF
  - pdfreader
  - pdfstamper
url: 1203.html
id: 1203
categories:
  - iTextSharp
date: 2009-06-24 07:00:00
---

![iTextSharp The Easy Way](/uploads/2009/06/ka_vol1_038Copy.jpg "iTextSharp The Easy Way") When I first started generating PDFs dynamically, I was overwhelmed by the complexity of the API.  Not just with iTextSharp, but it seemed that all of the APIs were complex. In looking through the API and comparing it to what I was actually trying to accomplish, I found there was a very small subset of classes and methods that I needed to use to accomplish the task at hand.  Now that I’ve learned more, I still use this same subset of commands for 90% of what I need to do in iTextSharp.  The reason we produce PDFs programmatically at all is because we need to dynamically generate some information on the page.  Most of the time, this information comes out of a database and gets placed on the same location of the page each time the page is generated.  The rest of the information is static. So what I normally do is have my designer or project manager create a PDF for me with form fields located where he wants the information to go.  Using the form fields, he can define the font, size, color, and position he wants to display the text with.  All I have to worry about is getting the text into the field. This works out nicely because once I’ve filled in the forms, he can move them around until he’s happy with them without asking for my help. We’ve already covered [setting the MIME type information](/2009/06/17/pdfs-using-itextsharp/) in our first post, so the rest of this discussion will assume you’ve already done that. The next thing you’ll want to do is load the PDF document that has the form fields in it and create a stamper object.  The stamper is what we use to grab the form fields object which we will use to set the form field values.

<!-- more -->

``` csharp
PdfReader pdfReader = new PdfReader(fileSpecifier);
PdfStamper stamp =
    new PdfStamper(pdfReader, Response.OutputStream);
AcroFields fields = stamp.AcroFields;
```

You’ll notice that the PdfStamper constructor takes two parameters.  The first is the PdfReader object we created above it.  The second is the Response.OutputStream.  This is how we can stream binary data back to the browser.  Make sure you use OutputStream and not Output.  Otherwise you’ll get a compiler error. Now, filling the form fields is just a matter of calling SetField().  The first parameter is the name of the field we want to set and the second parameter is the content we want to set it to.

``` csharp
fields.SetField("fieldName", "Field Content Goes Here")
```

The cool thing about this is that you can have multiple fields in your PDF with the same name and one SetField() call will set them both. Once you’ve filled all your fields, you “flatten” the form and send the information back to the browser.  If you choose to not flatten the form, you’ll end up with the form fields showing in the results.  Most of the time this is not what we had in mind.

``` csharp
stamp.FormFlattening = (true);
stamp.Close();
Response.End();
```
