---
title: Test Sending Email without a Server in ASP.NET
tags:
  - asp.net
  - 'c#'
  - email
  - mailaddress
  - mailmessage
  - smtpclient
  - testing
  - vb.net
url: 1382.html
id: 1382
categories:
  - ASP.NET
date: 2013-06-05 03:36:19
---

![back-041](/uploads/2009/08/back041.jpg "back-041") By now, most people are familiar with the fact that ASP.NET will send mail from the codebehind by simply adding a few lines to your web.config file and adding another few lines of code in the codebehind file.

But it wasn’t until recently that I found that you don’t need to have access to an SMTP server to test your code.

In fact, this little trick will allow you to read the email without clogging up your email client with email you only wanted for testing purposes.

Instead of the normal entry of

<mailSettings>
    <smtp from="you@domain.com">
        <network host="maiServerl" password="password" 
           userName="loginName" port="25"/>
    </smtp>
</mailSettings>

.csharpcode, .csharpcode pre { font-size: small; color: black; font-family: consolas, "Courier New", courier, monospace; background-color: #ffffff; /\*white-space: pre;\*/ } .csharpcode pre { margin: 0em; } .csharpcode .rem { color: #008000; } .csharpcode .kwrd { color: #0000ff; } .csharpcode .str { color: #006080; } .csharpcode .op { color: #0000c0; } .csharpcode .preproc { color: #cc6633; } .csharpcode .asp { background-color: #ffff00; } .csharpcode .html { color: #800000; } .csharpcode .attr { color: #ff0000; } .csharpcode .alt { background-color: #f4f4f4; width: 100%; margin: 0em; } .csharpcode .lnum { color: #606060; }

You can use

<mailSettings>
    <smtp from="you@domain.com"
         deliveryMethod="SpecifiedPickupDirectory">
        <specifiedPickupDirectory 
            pickupDirectoryLocation="c:\\mail"/>
    </smtp>
</mailSettings>

This will drop your email message in the c:\\mail directory as an *.eml file which you can open with Outlook Express.

The code you would write to send the mail is still the same:

SmtpClient smtp = new SmtpClient();
MailAddress from = new MailAddress(fromEmail, fromEmail);
MailAddress to = new MailAddress(emailAddress, emailAddress);
MailMessage message = new MailMessage(from, to);
message.Subject = SubjectLine;
message.Body = htmlString;
message.From = from;
message.To.Add(to);
message.IsBodyHtml = true;
smtp.Send(message);

.csharpcode, .csharpcode pre { font-size: small; color: black; font-family: consolas, "Courier New", courier, monospace; background-color: #ffffff; /\*white-space: pre;\*/ } .csharpcode pre { margin: 0em; } .csharpcode .rem { color: #008000; } .csharpcode .kwrd { color: #0000ff; } .csharpcode .str { color: #006080; } .csharpcode .op { color: #0000c0; } .csharpcode .preproc { color: #cc6633; } .csharpcode .asp { background-color: #ffff00; } .csharpcode .html { color: #800000; } .csharpcode .attr { color: #ff0000; } .csharpcode .alt { background-color: #f4f4f4; width: 100%; margin: 0em; } .csharpcode .lnum { color: #606060; }
