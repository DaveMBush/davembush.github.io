---
title: Image Map Control and JavaScript
tags:
  - image map
  - javascript
url: 157.html
id: 157
categories:
  - Javascript
date: 2008-05-19 02:45:17
---

Several months ago, I had to create a map that displayed push pins where various distributors were located.  When the user placed the cursor over the push pin, a tooltip was suppose to show up with a short form of the contact information in it. My first thought was, "hey, great, I can use that new ImageMap control they put in ASP.NET 2.0.  But, when I went to use it, I discovered quickly that you can't attach JavaScript to elements of the image map.  You can only attach them to the parent control which is nothing more than an image control with a collection of image map elements. Here's what I did...    **Other places talking about the ImageMap Control**

*   [ImageMap Control - ASP.Net 2.0](//asithangae.blogspot.com/2008/03/blog-post.html)

\- ImageMap is a New WebControl of ASP.Net 2.0. this is similar to the age old HTML control Map. This enables to set regions in a single image, and also sets a actions for that region. Means, if the region is clicked it will navigate to ...

*   [New ImageMap control in ASP.NET 2.0](//codebetter.com/blogs/darrell.norton/archive/2005/07/15/129250.aspx)

\- Here's a cool new control in ASP.NET 2.0, the ImageMap. You use it like this: <asp:imagemap id="Shop" imageurl="Images/ShopChoice.jpg" width="150" height="360" alternatetext="Shopping choices" runat="Server"> <asp:circlehotspot ...

*   [Build a custom hotspot for ImageMap](//weblogs.asp.net/dannychen/archive/2005/04/11/399873.aspx)

\- The reason for just these three shapes is that the image map standard only allow for these 3 shapes. Since polygon can create just about any shape we need, there certainly isn't any loss of flexibility. However, entering coordinates for ...