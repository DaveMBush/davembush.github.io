---
title: iTextSharp – HTML to PDF – Cleaning HTML
tags:
  - 'c#'
  - html
  - htmltidy
  - iTextSharp
  - PDF
url: 1302.html
id: 1302
categories:
  - iTextSharp
date: 2009-07-20 06:23:07
---

![H05K0013](/uploads/2009/07/H05K0013.jpg "H05K0013") The last prerequisite step prior to actually converting our HTML into PDF code is to clean up the HTML. The method I use takes advantage of the XML parser in .NET but in order to use that we have to have XHTML compliant XML. For this exercise, what I am most concerned about is that the HTML tags all have matching closing tags, that the tags are nested in a hierarchical structure, and that the tags all are lower case. Some of this we will have to rely on the user to provide, like properly nesting the tags.  But some of this we can attempt to clean up in our code.  If you know you will have complete control over your HTML, you might be able to skip this step.  But I think the code is simple enough that you’ll want to add it anyhow.  In my code, I have a function that accepts the HTML string and returns a collection of IElements that my main code will insert into the PDF.  The first thing I do in that function is make sure the code starts and ends with an open and close paragraph tag.  This is to ensure that I have at least one element that I can work with when I do the translation.

``` csharp
if (!xhtmlString.ToLower().StartsWith("<p>"))
    xhtmlString = "<p>" \+ xhtmlString + "</p>";
```

The next thing I do is make sure that all of the white space that isn’t the space character is removed from the code.

``` csharp
xhtmlString = xhtmlString
    .Replace("\
", string.Empty)
    .Replace("\\n", string.Empty)
    .Replace("\\t", string.Empty);
```

Then we want to change our BR tags to auto close.  Since I don’t deal with IMG tags in this code I don’t bother auto closing those tags.  If you decide to embellish this code to use the IMG tag, you’ll want to add code to fix that as well.

``` csharp
xhtmlString = xhtmlString
    .Replace("<BR>", "<br />")
    .Replace("<br>", "<br />");
```

Since my code currently ignores any attributes in the SPAN tag, I then remove the span tag’s attributes.

``` csharp
System.Text.RegularExpressions.Regex re = null;
System.Text.RegularExpressions.Match match = null;

re = new System.Text.RegularExpressions.Regex("<span.*?>");
match = re.Match(xhtmlString);
while (match.Success)
{
    foreach (System.Text.RegularExpressions.Capture c in match.Captures)
        xhtmlString = xhtmlString.Replace(c.Value, string.Empty);
    match = match.NextMatch();
}
```

Then I force all my tags to lower case

``` csharp
re = new System.Text.RegularExpressions.Regex("<\\\w+?");
match = re.Match(xhtmlString);

while (match.Success)
{
    foreach (System.Text.RegularExpressions.Capture c in match.Captures)
        xhtmlString = xhtmlString.Replace(c.Value, c.Value.ToLower());
    match = match.NextMatch();
}

re = new System.Text.RegularExpressions.Regex("</\\\w+?>");
match = re.Match(xhtmlString);
while (match.Success)
{
    foreach (System.Text.RegularExpressions.Capture c in match.Captures)
        xhtmlString = xhtmlString.Replace(c.Value, c.Value.ToLower());
    match = match.NextMatch();
}
```

Because the PDF code will treat each white space character as a character and HTML treats a string of white space characters as one space, I strip out any extra white space characters.  

``` csharp
while (xhtmlString.Contains("\> "))
    xhtmlString = xhtmlString.Replace("\> ", ">");
while (xhtmlString.Contains("  "))
    xhtmlString = xhtmlString.Replace("  ", " ");
```

And then I convert any special HTML strings to their text equivalent.  Right now, I only have to deal with the ampersand character.

``` csharp
xhtmlString = xhtmlString.Replace(" & ", " &amp; ");
```

Lastly, in order to ensure that my html string gets parsed correctly, I attempt to quote all my attributes

``` csharp
int length = 0;
while (length != xhtmlString.Length)
{
    length = xhtmlString.Length;
    xhtmlString = System.Text
        .RegularExpressions.Regex .Replace(xhtmlString,
        "(<.+?\\\s+\\\w+=)(\[^\\"'\]\\\S*?)(\[\\\s>\])", "$1\\"$2\\"$3");

}
```

With the exception of some features I’ve already noted that you might want to add, we’ve done all we can to clear up the code.  Any other problems are user input errors that will need to be corrected manually. Next, we can parse this HTML and convert it into PDF IElements. This process of cleaning up the HTML would all be a lot easier if HTML Tidy were converted to a managed code library.  (Yes, I know you can run it from .NET, but so far it is an external EXE, not managed code.)
