---
title: iTextSharp – HTML to PDF – Writing the PDF
tags:
  - 'c#'
  - fonts
  - html
  - iTextSharp
  - PDF
  - stack
url: 1330.html
id: 1330
categories:
  - iTextSharp
date: 2009-08-04 06:24:18
---

![B03B0085](/uploads/2009/08/B03B0085.jpg "B03B0085")

Last week we parsed the HTML and created code that keeps track of the various attributes we are going to need when we create the PDF.  Today we will finish the code and create the Elements that we can include in our PDF document.

One consideration we will need to keep in mind as we write out the PDF is that we have pushed various font characteristics that may overlap onto our stack.

To get the current font, we will need to combine the attributes.  For example, HTML that looks like this:

``` html
<p><u><i><b>this should be bold</b></i></u></p>
```

Should render as bold, italics, underlined text.  But we only pushed one element at a time, so if all we look at is the last element we pushed onto the stack, all we will get is a bold font.

To help with this, I created a helper method that does all the work of determining the correct current font and returning that to the caller.

The first part of the method does the bulk of the work.

``` csharp
string[] fontArray = fontCharacteristicStack.ToArray();
int fontIndex = 0;
fontNormalBoldItalic nbi = 0;
for (; fontIndex < fontArray.Length; fontIndex++)
{
    switch (fontArray[fontIndex].ToLower())
    {
        case "bold":
            nbi |= fontNormalBoldItalic.Bold;
            break;
        case "italic":
            nbi |= fontNormalBoldItalic.Italic;
            break;
        case "bolditalic":
        case "italicbold":
            nbi |= fontNormalBoldItalic.BoldItalic;
            break;
        case "underline":
            nbi |= fontNormalBoldItalic.Underline;
            break;
        case "boldunderline":
        case "underlinebold":
            nbi |= fontNormalBoldItalic.UnderlineBold;
            break;
        case "italicunderline":
        case "underlineitalic":
            nbi |= fontNormalBoldItalic.UnderlineItalic;
            break;
        case "underlinebolditalic":
        case "underlineitalicbold":
        case "boldunderlineitalic":
        case "bolditalicunderline":
        case "italicunderlinebold":
        case "italicboldunderline":
            nbi |= fontNormalBoldItalic.UnderlineBoldItalic;
            break;
    }
}
```

The fontNormalBoldItalic thing is an enumeration that I used to allow me to merge the font characteristics.

The second half gets the remainder of the font information which can be determined from the current element and applies the characteristics we determined above into the font.

``` csharp
Font font = FontFactory.getFont(currentFontName);
string s = FontFactory.TIMES;
switch (nbi)
{
    case fontNormalBoldItalic.Bold:
        font.setStyle(Font.BOLD);
        break;
    case fontNormalBoldItalic.Italic:
        font.setStyle(Font.ITALIC);
        break;
    case fontNormalBoldItalic.BoldItalic:
        font.setStyle(Font.BOLDITALIC);
        break;
    case fontNormalBoldItalic.Underline:
        font.setStyle(Font.UNDERLINE);
        break;
    case fontNormalBoldItalic.UnderlineBold:
        font.setStyle(Font.UNDERLINE | Font.BOLD);
        break;
    case fontNormalBoldItalic.UnderlineItalic:
        font.setStyle(Font.UNDERLINE | Font.ITALIC);
        break;
    case fontNormalBoldItalic.UnderlineBoldItalic:
        font.setStyle(Font.UNDERLINE | Font.BOLDITALIC);
        break;
}
font.setSize(currentFontSize);
if (currentFontColor.StartsWith("#"))
    font.setColor(System.Convert.ToInt32(currentFontColor.Substring(1, 2), 16),
        System.Convert.ToInt32(currentFontColor.Substring(3, 2), 16),
        System.Convert.ToInt32(currentFontColor.Substring(5, 2), 16));
else font.setColor(System.Drawing.Color.FromName(currentFontColor));
return font;
```

This is all called from our case statement when the element is text.

``` csharp
case XmlNodeType.Text:
    et = stack.Peek();
    Font font = getCurrentFont();
    if (et is Phrase)
        ((Phrase)(et)).add(
            new Chunk(reader.Value.
                Replace(" &amp; ", " & ").
                Replace("&nbsp;"," "), font));
    break;
```

You’ll notice that I’ve also added code at this point that translates the ampersand and the none breaking space so they render correctly in the PDF document.

Next time we address this topic we will try to close this all up with popping the attributes off the stack and dealing with the gaps between block elements that should (or should not) appear.
