---
title: 'I, J, and K Should Die'
tags:
  - naming conventions
  - programming
  - variables
url: 2525.html
id: 2525
categories:
  - Programming Best Practice
date: 2014-06-05 13:01:00
---

![ijk](/uploads/2014/05/ijk.png "ijk")One of the hardest things we do as programmers is naming things.  But the easiest thing to name is counter variables and most of us do it wrong several times a day. Of course, I’m talking about the notorious habit of naming our counter variables I, J, or K depending on how far down we’ve nested our looping.

<!-- more -->

History
-------

Now, my understanding of the history of I, J, and K as common variables that we use in programming is that the practice started in FORTRAN.  There are two reasons why FORTRAN programmers did this.  FORTRAN is primarily a math language.  I, J, and K were used in math and so the practice was carried over into FORTRAN and from there to other languages. FORTRAN encouraged this by also automatically declaring I, J and K as integers.  So using I, J and K made the code slightly easier to write and seemed to be considered a best practice at the time. The other reason I, J, and K have prevailed as indexing variables is because in the early days, our variable name length was limited and in some languages, the variable names were stored in our code and consumed memory, which was a precious resource we didn’t want to waste on long variable names. And so the practice continues to this day, largely because, “We’ve always done it that way.”

Story
-----

But let me tell you a story that illustrates why this is a bad idea. Admittedly, this story is pretty old, but only because in my 28 years of programming, this is still the best story I have to illustrate this issue. You see, one day, back when I was still regularly programming in C++ (for you kids out there, C++ is what Java and CSharp get most of their syntax) my manager came to me with a bug. Now the interesting thing about this bug is that it only occurred with one particular data set.  Most of the time it worked. So, I put on my detective’s hat and went to work.  Now, I try to go for the quick kill first.  So, I set break points at various locations and checked variables and found out …  nothing. That’s right, nothing. After about four hours of trying various methods of the above and trying to step through the code only to find the looping that was taking place was going to cause that to take several weeks, I finally stumbled on to the problem. And I do mean I stumbled, there’s no way I was going to find this problem using any of my standard debugging techniques. Here is what the code looked like:

``` csharp
for(i = 0;i < someValue;i++)
{
    // lots of code here
    for(j = 0;j < someOtherValue;j++)
    {
        // still more code here
        for(k = 0;k < yetAnotherValue;k++)
        {
            // and again, more code
            if(someConditionThatBarelyEverHappens)
            {
                for(i=0;i < 3;i++)
                {
                    // some correcting code here
                 }
             }
            // more code
         }
        // more code
    }
}
```

Do you see what I see?
----------------------

Do you see it? What was happening is that the inner “for(i…)” loop was resetting the outer I so that the whole loop essentially turned into an infinite loop for this data set. Now, this code was wrong on so many levels.  But the chief problem with the code is that by the time the original programmer for this routine got to the inner “for(i…)” loop they had forgotten that they’d already used “I” as a variable. For all I know, the guy who put the inner “for(i…)” loop in wasn’t even the same guy as the one who created the outer variable. So, how do we fix this code? Well that’s easy.  In fact, creating variable names that mean something in place of I, J, and K is one of the easiest code fixes we can implement. What is I and index of.  If you are looping through an index of bananas, call it “bananaIndex”  It isn’t rocket science.

Challenge
---------

And so, my code challenge to you is this:

*   Never, ever, use I, J or K (or any other single letter) as a variable name in your code.  There should be a rule in your code quality checking that just prevents this.  Even in JavaScript code.  With tools like closure compilers, there is no valid reason to use short variable names even in JavaScript.
*   When you find an existing single letter variable name, change it to something meaningful.

References
----------

*   [Wikipedia “Loop Counter” article](//en.wikipedia.org/wiki/Loop_counter)
*   [Code Complete](//www.amazon.com/gp/product/0735619670/ref=as_li_tl?ie=UTF8&camp=1789&creative=390957&creativeASIN=0735619670&linkCode=as2&tag=davmbusnetapp-20&linkId=BWBBIYB3LPS5JDEN)![](//ir-na.amazon-adsystem.com/e/ir?t=davmbusnetapp-20&l=as2&o=1&a=0735619670) – The book that first revealed this issue to me.

