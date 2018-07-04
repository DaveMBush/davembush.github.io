---
title: iTextSharp – HTML to PDF – Parsing HTML
tags:
  - asp.net
  - 'c#'
  - html
  - iTextSharp
  - parsing
  - PDF
  - xhtml
url: 1315.html
id: 1315
categories:
  - iTextSharp
date: 2009-07-28 07:03:54
---

![iStock_000004663193Medium](/uploads/2009/07/iStock_000004663193Medium.jpg "iStock_000004663193Medium")

Now that we have the HTML cleaned up, the next thing we will want to do is to parse the HTML.

In my actual code for this, I parse the HTML and create the PDF at the same time, but for the purposes of these posts, I’m going to deal primarily with parsing the HTML here and then deal with the PDF creation code later.

The key to parsing the HTML is that it is in XHTML form.  This allows us to use the XML APIs that are built into .NET.  For the purposes of parsing the HTML so that we can convert it to PDF code, we need to use the XMLTextReader.

Every time you Read() an XMLTextReader object, you will either be on a beginning tag, an ending tag, or text.  So the core of our loop looks something like this

XmlTextReader reader = 
    new XmlTextReader(xhtmlString, 
        XmlNodeType.Element, null);
while (reader.Read())
{
    switch (reader.NodeType)
    {
        case XmlNodeType.Element:
            // appropriate code break;
        case XmlNodeType.EndElement:
            // appropriate code break;
        case XmlNodeType.Text:
            // appropriate code break;
        case XmlNodeType.Whitespace:
            // appropriate code break;
    }

}

[](//11011.net/software/vspaste)where xhtmlString is the cleaned up HTML code from last week.

The core part of the translation is dependent on the fact that we have matching open and closing tags and that each time we hit an open tag, we can determine what the characteristics of that tag are.  Bold, underline, font, font size, etc.

So each time we hit the open tag, we will look up the characteristics.  For simplicity, I put this information in a resource file so that I could just look it up using code that looks something like this:

[](//11011.net/software/vspaste)

fontName = Resources.html2pdf .ResourceManager
    .GetString(tagName + "_fontName");

rather than having another long case statement in my code.

Once we have the information we want from the resource file, we place the current characteristics on a stack.  I created a different stack for each element, but in hindsight, it might have been better to create a structure with the information and use one stack of type in that structure.

Here’s the code that does that

if (!reader.IsEmptyElement)
{
    fontName = Resources.html2pdf.
        ResourceManager.
        GetString(tagName + "_fontName");
    if (fontName != null)
        currentFontName = fontName;
    fontSize = Resources.html2pdf.
        ResourceManager.
        GetString(tagName + "_fontSize");
    if (fontSize != null)
        currentFontSize = System.
            Convert.ToSingle(fontSize);
    fontColor = Resources.html2pdf.
        ResourceManager.
        GetString(tagName + "_fontColor");
    if (fontColor != null)
        currentFontColor = fontColor;
    fontCharacteristics = Resources.html2pdf.
        ResourceManager.
        GetString(tagName + "_fontCharacteristics");
    if (fontCharacteristics != null)
        currentFontCharacteristics = 
            fontCharacteristics;
}

[](//11011.net/software/vspaste)

Note that we only push the attributes of the element onto the stack if there is no content in the element.  This is because the closing node type will never be triggered on an element that has no content inside of it (BR and IMG tags, for example).

The final thing you’ll need to keep track of is if the element is a block element (P, DIV, etc) an inline tag (SPAN, A, etc) a list (OL,UL,LI), or even how much indentation is needed (primarily for list).

Frankly, the code for this was not fun to write.  Keep in mind too that there is nothing in here to handle special font characteristic attributes.  So your DIV tags can’t specify what font they should use or even how wide the font should be.  Not because it can’t be done, but because I have not had the need.

Here’s that code

strIndent = Resources.html2pdf.ResourceManager.GetString(tagName + "_indent");
isBlock = Resources.html2pdf.ResourceManager.GetString(tagName + "_isBlock");
string isList = Resources.html2pdf.ResourceManager.GetString(tagName + "_isList");
if (isBlock != null && isBlock.ToLower() == "true")
{
    string strIsList = 
        Resources.html2pdf .ResourceManager
        .GetString(tagName + "_isULList");
    
    if (strIsList != null && 
        strIsList.ToLower() == "true")
    {
        p = new List(false, 
            System.Convert .ToSingle(strIndent));
    }
    else {
        strIsList = Resources
            .html2pdf.ResourceManager
            .GetString(tagName + "isOLList");
        if (strIsList != null && strIsList.ToLower() == "true")
        {
            p = new List(true, 
                System.Convert.ToSingle(strIndent));
        }
        else {
            if (isList != null && isList.ToLower() == "true")
            {
                p = new iTextSharp.text.ListItem();
            }
            else {
                p = new Paragraph();
                ((Paragraph)p)
                    .SetLeading(m_leading, 1);
                if (stack.Count != 0)
                {
                    IElement e = stack.Pop();

                }
            }
        }
    }
    if (isList != null && isList.ToLower() == "true")
        ((iTextSharp.text.List)
            (list\[list.Count - 1\])).Add(p);
    else list.Add(p);
    stack.Push(p);
}

[](//11011.net/software/vspaste)

You’ll notice that there is a bit of code in here that deals with a p variable.  This code is needed so that if we are dealing with a block tag, we have a paragraph or list item to put the other content inside of the block when we hit it.  If we are dealing with an inline tag, we deal with that when we add the text.

Next week, we will show how to handle text and closing tags.
