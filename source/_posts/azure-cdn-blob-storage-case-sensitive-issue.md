---
title: Azure CDN Blob Storage Case Sensitive Issue
tags:
  - azure
  - blob storage
  - case sensitive
  - cdn
  - urlrewrite
url: 1963.html
id: 1963
categories:
  - ASP.NET
  - 'c#'
date: 2012-04-17 10:51:22
---

![misc_vol4_019](/uploads/2012/04/misc_vol4_019.jpg "misc_vol4_019") If you’ve done any work with Azure Blob storage, you already know that Blob storage is case sensitive.  If you’ve hooked the Azure CDN to blob storage, it is also case sensitive. You are probably reading this article because you’ve already run into this problem.  We ran into this problem when we were moving a site from an existing CDN to Azure.  The current site was on domain A and the resources hosted by the CDN were on domain B.  But the URLs to those resources were in various cases.  To make matters worse, there were multiple sites.  So it wasn’t a simple matter of just fixing up the case of one site. So we were looking for some way to make the CDN case insensitive, which you can’t do, when I finally thought, “Why don’t we make it so that it doesn’t matter with a response filter?”  I’ve done this before when I needed to make global changes and it worked well. Then I remembered, the URL Rewriting module for IIS7 does this for us.  And since this module is installed by default on Azure, all we needed to do was:

*   Make everything in blob storage lowercase
*   Install the redirect rules in web.config

Here is the web.config rules:

<rewrite>
  <outboundRules>
    <rule name="CSCAssets" preCondition="IsHtml"
        stopProcessing="true">
      <match filterByTags="Img"
         pattern="http://(www\\.)?domain\\.com/(.*)" />
      <action type="Rewrite"
         value="http://domain.com/{ToLower:{R:2}}" />
            <conditions logicalGrouping="MatchAny" />
    </rule>
    <preConditions>
      <preCondition name="IsHtml">
         <add input="{RESPONSE\_CONTENT\_TYPE}"
            pattern="^text/html" />
      </preCondition>
        </preConditions>
      </outboundRules>
    </rewrite>

This will force any URL pointing to [http://www.domain.com](//www.domain.com) or [http://domain.com](//domain.com) to point to [http://domain.com](//domain.com) and force it all to lowercase on the way back to the client’s browser.
