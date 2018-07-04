---
title: ASP.NET Cross Domain Form Submission
tags:
  - asp.net
  - cross domain
  - form
  - javascript
  - subform
  - submit
url: 1519.html
id: 1519
categories:
  - ASP.NET
date: 2009-11-11 08:47:36
---

![G03A0021](/uploads/2009/11/G03A0021.jpg "G03A0021")

Not to be confused with cross page posting, cross domain submission allows us to post the contents of an ASP.NET form to a completely different domain.

To achieve this we will need to use a bit of javascript and you’ll need to resort to using regular HTML controls.

We will cover two cases.  The first is pretty easy.  You just need a form on your screen that will allow you to send the information to another domain.  The second will be more complex.  You need a form on your screen that you fill from your domain prior to sending to the second domain.

To start, create a new ASPX page.  You can use a master page if you want.  It won’t make much difference.

Because ASP.NET has it’s own FORM tags, to add another form tag into the output, you’ll want to close off the current form tag and start another.  The best way to do this is with a literal control.  This way the ASP.NET runtime engine will not be confused and any of the other runat=server controls you have on the page after this new form will continue to behave as expected.  However, you’ll want to make sure that any controls that produce input tags of their own all appear prior to the new form tag.

<%\-\- It is best if all your pages controls exist 
    prior to the new form here --%>
<asp:Literal ID="Literal1" runat="server" Text=
    '</form><form name="subform" action="newDomainAndPage" method="POST">'
    ></asp:Literal>
 <input type="text" name="someName" />
 <input type="submit" name="submit" value="Submit" />

The submit button will now post to the site specified in “newDomainAndPage”

If all of the information on the page is going to go to the new domain, things get a little easier.  You won’t need this sub form.  But, you will need a small snippet of javascript to change the action attribute in the form tag that .NET creates for you.

<script type="text/javascript" language='javascript'>
document.remote.action=
    "https://someotherdomain.com/someotherpage";
</script>

.csharpcode, .csharpcode pre { font-size: small; color: black; font-family: consolas, "Courier New", courier, monospace; background-color: #ffffff; /\*white-space: pre;\*/ } .csharpcode pre { margin: 0em; } .csharpcode .rem { color: #008000; } .csharpcode .kwrd { color: #0000ff; } .csharpcode .str { color: #006080; } .csharpcode .op { color: #0000c0; } .csharpcode .preproc { color: #cc6633; } .csharpcode .asp { background-color: #ffff00; } .csharpcode .html { color: #800000; } .csharpcode .attr { color: #ff0000; } .csharpcode .alt { background-color: #f4f4f4; width: 100%; margin: 0em; } .csharpcode .lnum { color: #606060; }

where “remote” is the ID of the form in your form tag when it is rendered.  If you are using a master page, the form tag will be mangled.  You’ll either need to generate the javascript in your codebehind and grab the form tag’s ClientID or you’ll need to do a view source and see what it got changed to.  Using ClientID is preferred because the ID could change over time (say, when a new version of .NET comes out).
