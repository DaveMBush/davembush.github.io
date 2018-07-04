---
title: Embedding Google Search Appliance Results in ASP.NET
tags:
  - asp.net
  - 'c#'
  - google
  - googxslt.xsl
  - gsa
  - httpwebrequest
  - httpwebresponse
  - search
url: 1509.html
id: 1509
categories:
  - ASP.NET
date: 2009-11-05 08:13:35
---

![C03H0075](/uploads/2009/11/C03H0075.jpg "C03H0075")

Several of the projects I’m involved with use the Google Search Appliance for their search engine.  For each of these projects, we’ve wanted to integrate the results on an ASPX page so that the results look like they are part of the site rather than taking them to another site to display the results.  This is achieved by using the XML Control, the Google XSLT file, and some good old-fashioned search and replace.

It has been a while since I’ve actually worked on this code.  Mostly we just copy, paste, and modify from one project to another, but I hope to give you enough here to at least get you started doing this yourself, if not everything you need.

On the ASPX page that you want to display your search results on, put an XML Control.  This is the only control you will need on the page for this to work.

Any place you have a search box on your site, you’ll need to create a button to process the search field.  I recommend putting the text box and the button in a panel so that you can make the button the default button for the contents of the panel.

In the click event handler you will redirect to your search results page and you’ll need to either specify the Google search appliance parameters here or when you actually access the Google appliance in the search results page.  I do it during the redirect.

My code looks something like this

String q = string.Empty;
q = Server.UrlEncode(m_textBoxSearch.Text);
if (q.Length > 0)
{
    string qsAppend = "&btnG=Google+Search&site=" \+ 
        "\[sitename\]&client=\[client\]&output=xml\_no\_dtd";
    Response.Redirect("~/Search.aspx?q=" \+ q + qsAppend);
}

.csharpcode, .csharpcode pre { font-size: small; color: black; font-family: consolas, "Courier New", courier, monospace; background-color: #ffffff; /\*white-space: pre;\*/ } .csharpcode pre { margin: 0em; } .csharpcode .rem { color: #008000; } .csharpcode .kwrd { color: #0000ff; } .csharpcode .str { color: #006080; } .csharpcode .op { color: #0000c0; } .csharpcode .preproc { color: #cc6633; } .csharpcode .asp { background-color: #ffff00; } .csharpcode .html { color: #800000; } .csharpcode .attr { color: #ff0000; } .csharpcode .alt { background-color: #f4f4f4; width: 100%; margin: 0em; } .csharpcode .lnum { color: #606060; }

A couple of thing to make sure you notice.  Make sure you URLEncode the search text and make sure you specify the output as xml\_no\_dtd.  This is what will allow us to embed the results on the search results page.

For the search results, the codebehind is where all the work is done.

During the page load event, you’ll process the query string that is passed in, forward it on to Google, and then place the results in the XML control.  It is pretty straightforward, but there are a few “gotchas” along the way.

Here’s my code

String searchResults = String.Empty;
String url = String.Format 
    ("http://goo.gle.ip.addr/search?{0}", 
    Request.QueryString.ToString());

HttpWebResponse objResponse;
HttpWebRequest objRequest = 
    (HttpWebRequest) HttpWebRequest.Create (url);
objRequest.ReadWriteTimeout = 90000;

objRequest.UserAgent = Request.UserAgent;
objRequest.UserAgent += String.Empty;
objResponse = 
    (HttpWebResponse) objRequest.GetResponse ();

using (StreamReader sr = new 
   StreamReader (objResponse.GetResponseStream ()))
{
    char \[\] buffer = new char \[200000\];
    sr.ReadBlock (buffer, 0, 200000);
    searchResults = new string (buffer);
    searchResults = searchResults.Substring (0, 
        searchResults.IndexOf ("</GSP>") \+ 6);
    sr.Close (); // Close and clean up the StreamReader
}

if (String.IsNullOrEmpty (searchResults) == false)
{
    searchResults = searchResults
        .Replace ("search?", "Search.aspx?");
    String styleSheet = 
        Server.MapPath ("~/templates/GoogXSLT.xsl");

    if (File.Exists (styleSheet) == true)
    {
        m_xGoogResults.Document =
            new System.Xml.XmlDocument ();
        m_xGoogResults.Document
            .LoadXml (searchResults);
        m_xGoogResults.TransformSource = 
            styleSheet;
    }
}

.csharpcode, .csharpcode pre { font-size: small; color: black; font-family: consolas, "Courier New", courier, monospace; background-color: #ffffff; /\*white-space: pre;\*/ } .csharpcode pre { margin: 0em; } .csharpcode .rem { color: #008000; } .csharpcode .kwrd { color: #0000ff; } .csharpcode .str { color: #006080; } .csharpcode .op { color: #0000c0; } .csharpcode .preproc { color: #cc6633; } .csharpcode .asp { background-color: #ffff00; } .csharpcode .html { color: #800000; } .csharpcode .attr { color: #ff0000; } .csharpcode .alt { background-color: #f4f4f4; width: 100%; margin: 0em; } .csharpcode .lnum { color: #606060; }

Notice that the first thing we do is to form a URL that we can pass to the Google search appliance.  Then, using the HttpWebResponse and HttpWebRequest objects, we retrieve the search results from Google.  I set the UserAgent string to the same information that is in the calling browser just in case there is some browser specific code that is being returned (doubt it, but why not?).

You’ll also notice some funky code looking for the closing </GPS> string.  This is one of the gotchas.  For some reason, Google returns (or did when I wrote this code) extra bytes past the closing tag that the XML control can’t deal with.  This search/substring code cuts those extra characters off.

The final bit of code is some fix-up and loading into the XML control.  I might have modified the GoogXSLT.xsl file that Google gave us.  But I really don’t like modifying source from an external source that might change if I can help it at all.  So, what the search and replace does is that it looks for the “search?” string that is going to be in the results and changes it so that it will redirect to my search results page, named “search.aspx”.

**Other places talking about coding the Google Search Appliance In ASP.NET**

I couldn’t find any.  If you know of any, let me know in the comments.
