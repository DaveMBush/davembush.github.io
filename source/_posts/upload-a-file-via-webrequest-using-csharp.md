---
title: Upload a File via WebRequest Using CSharp
tags:
  - 'c#'
  - contenttype
  - file upload
  - multipart/form-data
  - post
  - webrequest
  - webresponse
url: 1517.html
id: 1517
categories:
  - 'c#'
date: 2009-11-10 09:15:52
---

![G03A0085](/uploads/2009/11/G03A0085.jpg "G03A0085")

I got this question a couple of weeks ago but just never had the time to put into answering fully.  But today I have some extra time due to the fact that I’m under-booked with projects.

The question went something like this:

“I want to be able to upload a file from a desktop application to a web site that has a form that accepts the file as a post.  How do I do that?”

And while I’ve done some things in the past that come close, I’ve never had to do this exact task.  But it does look interesting.

The entire trick to making this work is to make the web server think that the form has been filled out and that the post has been implemented.

To do this, you’ll need to create a WebRequest object and set some properties on it.  The two that you must set are the Method property (to POST) and the ContentType (to “multipart/form-data; boundary=”+boundary).  If you are sending multiple files, you’ll need multiple boundaries.

System.Net.WebRequest wr = 
    System.Net.WebRequest.Create("url goes here");
wr.ContentType = "multipart/form-data; boundary=xyz";
wr.Method = "POST";

.csharpcode, .csharpcode pre { font-size: small; color: black; font-family: consolas, "Courier New", courier, monospace; background-color: #ffffff; /\*white-space: pre;\*/ } .csharpcode pre { margin: 0em; } .csharpcode .rem { color: #008000; } .csharpcode .kwrd { color: #0000ff; } .csharpcode .str { color: #006080; } .csharpcode .op { color: #0000c0; } .csharpcode .preproc { color: #cc6633; } .csharpcode .asp { background-color: #ffff00; } .csharpcode .html { color: #800000; } .csharpcode .attr { color: #ff0000; } .csharpcode .alt { background-color: #f4f4f4; width: 100%; margin: 0em; } .csharpcode .lnum { color: #606060; }

To send data, we need to format the data in multipart/form-data mode.  First we retrieve the file into a stream.

FileStream fs= new FileStream("your file here",
    System.IO.FileMode.Open, System.IO.FileAccess.Read);
byteArray= new byte\[loFile.Length\];
fs.Read(byteArray,0,(int) byteArray.Length);
fs.Close();

.csharpcode, .csharpcode pre { font-size: small; color: black; font-family: consolas, "Courier New", courier, monospace; background-color: #ffffff; /\*white-space: pre;\*/ } .csharpcode pre { margin: 0em; } .csharpcode .rem { color: #008000; } .csharpcode .kwrd { color: #0000ff; } .csharpcode .str { color: #006080; } .csharpcode .op { color: #0000c0; } .csharpcode .preproc { color: #cc6633; } .csharpcode .asp { background-color: #ffff00; } .csharpcode .html { color: #800000; } .csharpcode .attr { color: #ff0000; } .csharpcode .alt { background-color: #f4f4f4; width: 100%; margin: 0em; } .csharpcode .lnum { color: #606060; }

Then we setup the multipart boundary header.

string var = "--" \+ boundaryID + "\
\\n"  +
     "Content-Disposition: form-data; name=\\"" \+ Key + "\\" filename=\\"" +
      new FileInfo("your file here").Name + "\\"\
\\n\
\\n";

Then we write the boundary header and the content to the output stream .csharpcode, .csharpcode pre { font-size: small; color: black; font-family: consolas, "Courier New", courier, monospace; background-color: #ffffff; /\*white-space: pre;\*/ } .csharpcode pre { margin: 0em; } .csharpcode .rem { color: #008000; } .csharpcode .kwrd { color: #0000ff; } .csharpcode .str { color: #006080; } .csharpcode .op { color: #0000c0; } .csharpcode .preproc { color: #cc6633; } .csharpcode .asp { background-color: #ffff00; } .csharpcode .html { color: #800000; } .csharpcode .attr { color: #ff0000; } .csharpcode .alt { background-color: #f4f4f4; width: 100%; margin: 0em; } .csharpcode .lnum { color: #606060; }

System.IO.MemoryStream ms = new System.IO.MemoryStream();
System.IO.BinaryWriter bw = new System.IO.BinaryWriter(ms);
bw.Write(Encoding.GetEncoding(1252).GetBytes(var));
bw.Write(byteArray);
bw.Write(Encoding.GetEncoding(1252).GetBytes("\
\\n"));

System.IO.Stream postStream = wr.GetRequestStream();
ms.WriteTo(postStream);
postStream.Close();

.csharpcode, .csharpcode pre { font-size: small; color: black; font-family: consolas, "Courier New", courier, monospace; background-color: #ffffff; /\*white-space: pre;\*/ } .csharpcode pre { margin: 0em; } .csharpcode .rem { color: #008000; } .csharpcode .kwrd { color: #0000ff; } .csharpcode .str { color: #006080; } .csharpcode .op { color: #0000c0; } .csharpcode .preproc { color: #cc6633; } .csharpcode .asp { background-color: #ffff00; } .csharpcode .html { color: #800000; } .csharpcode .attr { color: #ff0000; } .csharpcode .alt { background-color: #f4f4f4; width: 100%; margin: 0em; } .csharpcode .lnum { color: #606060; }

Note the last bw.Write() line is telling the server this is the  end of the boundary.

And then the final bit of code that sends it to the server and gets the resulting HTML.

System.Net.WebResponse wresponse = wr.GetResponse();
System.Text.Encoding enc = 
    System.Text.Encoding.GetEncoding(1252);
System.IO.StreamReader responseStream =
    new System.IO.StreamReader
        (wresponse.GetResponseStream(), enc);
string lcHtml = responseStream.ReadToEnd();
wresponse.Close();
responseStream.Close();

.csharpcode, .csharpcode pre { font-size: small; color: black; font-family: consolas, "Courier New", courier, monospace; background-color: #ffffff; /\*white-space: pre;\*/ } .csharpcode pre { margin: 0em; } .csharpcode .rem { color: #008000; } .csharpcode .kwrd { color: #0000ff; } .csharpcode .str { color: #006080; } .csharpcode .op { color: #0000c0; } .csharpcode .preproc { color: #cc6633; } .csharpcode .asp { background-color: #ffff00; } .csharpcode .html { color: #800000; } .csharpcode .attr { color: #ff0000; } .csharpcode .alt { background-color: #f4f4f4; width: 100%; margin: 0em; } .csharpcode .lnum { color: #606060; }

That’s all there is to it.  Once you know how to do it, it is rather simple.

**Other places talking about WebRequest using multipart/form-data**

[Brian Grinstead » Blog Archive » Multipart Form Post in C#](//www.briangrinstead.com/blog/multipart-form-post-in-c) \- So, I needed to roll my own form post. Here is the Multipart Form RFC and the W3C Specification for multipart/form data. After reading these links and searching some forums, here is what I came up with. ..... userAgent, contentType, formData) End Function Private Function PostForm(ByVal postUrl As String, _ ByVal userAgent As String, _ ByVal contentType As String, _ ByVal formData As Byte()) _ As HttpWebResponse Try Dim request As HttpWebRequest = WebRequest. ...

[Retrieving HTTP content in .NET with WebRequest/WebResponse](//www.west-wind.com/presentations/dotnetWebRequest/dotnetWebRequest.htm) \- This code finalizes the request for POST data by checking whether we've already written something into the POST buffer and if so configuring the POST request by specifying the content type. In the case of multi-part form POST an ...
