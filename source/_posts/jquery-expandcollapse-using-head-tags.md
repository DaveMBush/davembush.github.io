---
title: jQuery Expand/Collapse Using Head Tags
tags:
  - collapse
  - dotnetnuke
  - expand
  - html
  - javascript
  - jQuery
url: 1470.html
id: 1470
categories:
  - jQuery
date: 2013-09-25 12:05:40
---

![animal-010](/uploads/2009/10/animal010.jpg "animal-010")

I’ve spent a good chunk of the last two days working on an interesting project for one of my clients that I think the rest of the jQuery community could benefit from.

The task started when my client came to me with an existing script that was being used in a DotNetNuke system to expand and collapse content under head tags that was produced by an article editing system similar to the Text/HTML module.

<!-- more -->

The problem with the way the author had created the system is that the content author would have had to add DIV tags to the content and if any additional sections were added to the content the jQuery and CSS code would have to be modified.

The question that was posed to me was, “Is there any way of protecting the code that is relevant to the jQuery so that the content authors can’t mess it up?”  My response was basically, No, but if the jQuery was written correctly, that shouldn’t be an issue.  You should be able to have the jQuery add the additional DIV tags for you so that the content authors can just concentrate on writing the article.

Of course, he got all excited about that possibility along with the fact that he’d be able to re-use the code.

One final note.  We decided to use H4, H5, and H6 tags so that if the user didn’t want the expand collapse functionality for a particular section they could use H1, H2, and H3 for that purpose.

The first thing we needed to do was place a DIV tag around the content we want to have impacted by our code.  I’m using the class HEC to indicate this.

In DotNetNuke, there are a bunch of DIV tags that are ultimately surrounding the first tag I want to impact and some other extraneous DIV tags at the bottom that got in the way of the presentation, so rather than creating a selector that looked for the content within the HEC class, I’m looking for the content that is within the parent of the H4 element that is within the HEC class.

Since the page may have  multiple sections of HEC classes, I iterate through each parent that is found and start a search and replace to add additional DIV tags to the markup.  This allows me to place a DIV around the content that is between each H4 tag and another DIV around the group of H4 and DIV tags that go together.

I also place open DIV tags at the top of the HTML and closing DIV tags at the end so that we end up with HTML that is as valid as what we started out with.

For subsections, I grab the first sibling of each H4 tag (and ultimately H5 tag) and change its HTML as well, similar to how I changed the parent elements.

You’ll notice by looking at the code below that the DIV tags are classed so that you can apply a stylesheet to them.

The last bit of information is that I insert instructional information and an expand-all and collapse-all button/link prior to the first H4 tag.

All of this work allows the content authors to write using a regular WYSIWYG HTML editor without having to worry about adding none visible markup to make it display correctly.

Here’s a snippet of the source:

``` javascript
$(function() {
    var strHeader = "<div class='showHideControls'><span class='showAll showHide'>Expand All</span> <span class='hideAll showHide'>Collapse All</span></div>" +
        "<div class='instructions-arrow'>Click arrow to view step details</div>";
    var strHtml;
    $(".HEC h4").parent().each(function() {
        strHtml = $(this).html();
        strHtml = strHtml.replace(/<\\/h4>/gi, "</h4><div class='content'>");
        strHtml = strHtml.replace(/<h4/gi, "</div></div><div class='accordian'><h4");
        strHtml = "<div><div>" \+ strHtml + "</div></div>";
        $(this).html(strHtml);
        $("h4").bind("click", function(event) {
            $(this).next().slideToggle("slow");
            return false;
        });
    });

// to implement the sub sections, you would select the
// div that is next() to the header and apply similar
// search and replace logic

// start in collapsed mode
 $(".HEC .content").hide();

    $(".HEC .accordian:first").before(strHeader);
    $(".HEC .showAll").bind("click", function() {
        $(".HEC .content").show();
        // other .show()s go here for the subsections
    });
    $(".HEC .hideAll").bind("click", function() {
        $(".HEC .content").hide();
        // other hide()s go here for the subsections

    });

});
```
