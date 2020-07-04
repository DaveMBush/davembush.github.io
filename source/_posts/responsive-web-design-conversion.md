---
title: Responsive Web Design Conversion
tags:
  - css
  - html
  - javascript
  - responsive design
url: 2788.html
id: 2788
categories:
  - CSS
date: 2014-12-25 07:00:00
---

Several weeks ago, I started the process of converting this blog to a responsive design.  At this point it is mostly done.  But it is done enough that I can tell you the process that I went through to get the site converted. I was surprised by how easy the process was.  But I guess I was lucky because the theme I started out with already was following a number of best practices. So, to start out with, if you want to move your site toward responsive web design, there are some prerequisites that you’ll need to pay attention to.

<!-- more -->

Responsive Design – Prerequisites
---------------------------------

First, before you do anything else, you need to make sure your current theme is not using tables anywhere.  If you are, the first thing you will need to do is to convert your them from tables to using DIVs.  The CSS world has been telling you this for years.  And now you see why this is true. Frankly, if you are using tables instead of DIVs for your layout, you are probably better off just starting over. Second, you need to ensure that you don’t have any CSS embedded in your HTML, or even worse, style attributes in your elements that are going to get in the way of your design.  While it is true, you can override these with JavaScript, the less of this you have to do, the better.  So clean that up now. Third, you need to think about what you really NEED to show on the page and what can be  hidden.  Sidebars are a good candidate for hiding since they aren’t your main content.  On this site I have two side bars.  One on the left and one on the right.  I’ve decided that the right sidebar is more important, so I’ve hidden the left one for small screens and both for tiny screens.  You might decide to move them to the bottom instead.  But you might find that more difficult than hiding them.

Responsive Web Design – HTML
----------------------------

With the prerequisites taken care of, let’s look at a small HTML change you will need to make. You will need to add the following META tag into the HEAD area of your HTML:

``` html
<meta name="viewport"
    content="width=device-width, initial-scale=1">
```

I’ll let you search for the technical explanation of what this actually does.  What I’ll tell you is why you need it. If you don’t add this then when you go to view your web site on the iPhone and other browsers that are looking for this tag, you’ll see the full site scaled down to fit on the phone instead of the adaptive rendering you were probably expecting. That is, you’ll have everything working on your desktop so that it works on various screen sizes as you expect, but when you try to render it on your phone, it will scale the full version of the site to fit within the screen.

Responsive Web Design – CSS
---------------------------

Most of your changes will be in the CSS.  This is where a lot of the magic happens.  The first change you are going to want to make to your original design is that you are going to want to make all of your width information percentage values instead of hard pixel values (or EMs, if that’s what you use.) Of course your outer DIV is going to need to be 100%.  But if you do that, and you view your site on a large monitor, your site is going to be really hard to read.   So, to fix that issue, add the max-width attribute to the css definition for that element.

``` css
.container{
   width: 100%;
   max-width: 1200px;
}
```

And now we have to add rules to tell the browsers how to render the page at different resolutions.  To do that, we use media tags. You may have thought that media tags were just for styling the printed version of your web site, or for telling text to speech engines how to read your page.  But they are also useful for telling the browser what rules to use for the various screen resolutions. You can use media tags for screen in the same way that you use media tags for print.  Either by specifying in your CSS or by specifying  in your LINK statement in the HTML.  I choose to put them in my CSS because I wasn’t adding that much information and I compress my CSS anyhow. The basic syntax looks like:

``` css
@media screen and (some rule here)
```

It is the rule bit that gets confusing. The rule bit looks like:

``` css
@media screen and (max-width: 600px)
```

What this says is, only apply this CSS to screen sizes LESS THAN or equal to 600px.  That is, only apply the CSS to a screen who’s max width is 600px. If you are going to mess up anything, this is where it will be. there is also a rule for min-width, so you can say something like

``` css
@media screen and (max-width: 600px) and (min-width:300px)
```

Which would only apply the CSS to a screen width between 300 and 600 pixels. Now, you might be inclined to use a rule like that, but what I’ve found is that most of my CSS is additive.  That is, the rules I applied to max-with of 900 pixels I also want applied to max-width of 600 pixels.  I just want additional rules applied to max-width of 600.  So, I tend to just stack them instead of making a separate rule set for each resolution. Other than removing the sidebars, you may also want to adjust font sizes and banner sizes based on the size of the screen.  That’s what I did here.  You’ll notice that the banner area shrinks as the screen gets smaller.

Responsive Web Design – JavaScript
----------------------------------

OK, so now you have your screens scaling nicely, but you may notice one final issue.  All of those images you have on your site are all a fixed size.  Some of those you might be able to fix by using CSS.  But, in my case the images I had the most trouble with were the images that show up at the top of the post.  So, in my case, I wrote some JavaScript to scale them to fit the width of the container.  I still have to modify the code in some of my older posts to work better with the JavaScript I wrote.  But for the newer posts, this is all working well.  I’ve been thinking about making the scaling code work as the screen size changes, but that’s a lot of work for something most people aren’t going to use. If you do decide to change the screen size and you see the images bleeding over the width of the container, refresh your browser and the images will scale.
