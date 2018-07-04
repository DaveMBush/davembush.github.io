---
title: Test Driven Development Kata - Roman to Arabic in JavaScript
tags:
  - javascript
  - kata
  - tdd
url: 3992.html
id: 3992
categories:
  - TDD
date: 2016-07-20 06:30:00
---

Coding Katas are a way of developing your skills as a programmer.  I thought it might be informative to tackle one of the classics as a blog post. Depending on how this works, I may or may not do another one quite so publicly. The rules I’m going to try to adhere by.

1.  I will document what I am doing as I go.
2.  This is not a pre-coded blog post.  You’ll get to “see” me code as I go.
3.  I will write all tests first.
4.  I will only write enough code to make the current tests succeed.

  ![Test Driven Kata - Roman to Arabic](/uploads/2016/07/image-3.png "Test Driven Kata - Roman to Arabic") 

Today’s problem:
----------------

This is a pretty classic coding problem that shows up in interviews, home-work assignments, and code katas. As an interview problem, I find it lacking because it typically does not represent the kind of work you will be doing other than proving that you can solve problems in your chosen language.  It is also a standard coding problem, meaning the person interviewing you is using the most obvious problem to see if you can code.  Much like asking "What is your greatest strength?" and "What is your greatest weakness?"  Finally, it takes longer to complete than I believe interview coding problems should.  This is not to say, I don't jump through these hoops myself. As a kata exercise, it is good because there are several ways you might solve the problem.  And for our purposes, it also demonstrates what Test Driven Development might look like using JavaScript. So, what is the problem? Write a function that converts Roman numbers into Arabic numbers and throws an error if the Roman number is in an invalid form. That sounds pretty easy.  But the first question we need to ask is, “What, exactly are the rules for converting Roman numbers into Arabic numbers?”

1.  The values for Roman numbers are as follows:
    
    Roman
    
    Arabic
    
    I
    
    1
    
    V
    
    5
    
    X
    
    10
    
    L
    
    50
    
    C
    
    100
    
    D
    
    500
    
    M
    
    100
    
2.  Repeating a number up to three times adds that value three times.
3.  A Roman ‘digit’ can’t repeat more than three times.  Instead the previous 1, 10, or 100 equivalent value is used to subtract from the next ‘digit’.  That is,
    
    Roman
    
    Arabic
    
    IV
    
    4
    
    IX
    
    9
    
    XL
    
    40
    
    XC
    
    90
    
    CD
    
    400
    
    CM
    
    900
    
4.  With the exception of the subtraction rule above, all values must decrease in scale from left to right and are added together.

Test 1
------

Since we will be using JavaScript, the testing framework we will be using is Jasmine. Our first test will simply test that when we pass in “I” we get back 1. Since there is no way of embedding GitHub repository files into WordPress that I know of, and I’m too lazy to create a gist for each step along the way, I’ll just provide links to the source at the various points along the way.  If you know of a way to directly include the source files, let me know in the comments. So, here is the first link: [https://github.com/DaveMBush/JavaScriptKatas/blob/Step-1/tests/roman-to-arabic/romanToArabic.spec.js](//github.com/DaveMBush/JavaScriptKatas/blob/Step-1/tests/roman-to-arabic/romanToArabic.spec.js "https://github.com/DaveMBush/JavaScriptKatas/blob/Step-1/tests/roman-to-arabic/romanToArabic.spec.js") And the code that passes this test is here: [https://github.com/DaveMBush/JavaScriptKatas/blob/Step-1/katas/roman-to-arabic/romanToArabic.js](//github.com/DaveMBush/JavaScriptKatas/blob/Step-1/katas/roman-to-arabic/romanToArabic.js "https://github.com/DaveMBush/JavaScriptKatas/blob/Step-1/katas/roman-to-arabic/romanToArabic.js") You might think, what’s the point?  Why just return 1 when you know you are going to have to do more?  Well, when you are doing TDD, you have to work off of what you are testing for now, not what you might test for later.  So, we return 1.

Test 2
------

From here, we can go a couple of different directions.  How about testing for rule 3 next.  How would we do that? To start with, we might just make sure that if we pass in IV, we get back 4.  Now our code is getting a bit more complicated.  But sticking with our TDD principles, we’ll just test for IV.  Another happy path test. Here is the code that test for our two conditions: [https://github.com/DaveMBush/JavaScriptKatas/blob/Step-2/tests/roman-to-arabic/romanToArabic.spec.js](//github.com/DaveMBush/JavaScriptKatas/blob/Step-2/tests/roman-to-arabic/romanToArabic.spec.js "https://github.com/DaveMBush/JavaScriptKatas/blob/Step-2/tests/roman-to-arabic/romanToArabic.spec.js") And the code that implements it [https://github.com/DaveMBush/JavaScriptKatas/blob/Step-2/katas/roman-to-arabic/romanToArabic.js](//github.com/DaveMBush/JavaScriptKatas/blob/Step-2/katas/roman-to-arabic/romanToArabic.js "https://github.com/DaveMBush/JavaScriptKatas/blob/Step-2/katas/roman-to-arabic/romanToArabic.js")

Test 3
------

OK.  What happens if IV shows up twice?  That should be a failure.  IV should never show up more than once.  In fact, none of the codes that show up in rule three should show up more than once.  Let’s make sure they don’t. The tests for this will be pretty simple, and to save time, we will code for all of them at once.  In fact, we are going to eventually need the conversion tables above, so let’s go ahead and put them in now. Here is what our test file looks like now.  Notice that we’ve put several tests in at once. Notice that I didn’t have to write multiple tests to do this.  I just created one test and iterated over it. [https://github.com/DaveMBush/JavaScriptKatas/blob/Step-3/tests/roman-to-arabic/romanToArabic.spec.js](//github.com/DaveMBush/JavaScriptKatas/blob/Step-3/tests/roman-to-arabic/romanToArabic.spec.js "https://github.com/DaveMBush/JavaScriptKatas/blob/Step-3/tests/roman-to-arabic/romanToArabic.spec.js") And the solution also iterates. [https://github.com/DaveMBush/JavaScriptKatas/blob/Step-3/katas/roman-to-arabic/romanToArabic.js](//github.com/DaveMBush/JavaScriptKatas/blob/Step-3/katas/roman-to-arabic/romanToArabic.js "https://github.com/DaveMBush/JavaScriptKatas/blob/Step-3/katas/roman-to-arabic/romanToArabic.js")

Test 4
------

Just to make sure we aren’t counting how many times in a row this all shows up, let’s add a valid Roman Number in between. [https://github.com/DaveMBush/JavaScriptKatas/blob/Step-4/tests/roman-to-arabic/romanToArabic.spec.js](//github.com/DaveMBush/JavaScriptKatas/blob/Step-4/tests/roman-to-arabic/romanToArabic.spec.js "https://github.com/DaveMBush/JavaScriptKatas/blob/Step-4/tests/roman-to-arabic/romanToArabic.spec.js") Look at that, it all still works!

Test 5
------

Next, we need to make sure that none of the items in our base table show up more than 3 times.  We’ll write a similar test to what we’ve already written with a twist.  If any of those numerals show up more than 4 times, we know we have a problem either because they are out of order or because they show up all in a row.  So, we only need to check the count.  If there are four of any of them, we have a problem. [https://github.com/DaveMBush/JavaScriptKatas/blob/Step-5/tests/roman-to-arabic/romanToArabic.spec.js](//github.com/DaveMBush/JavaScriptKatas/blob/Step-5/tests/roman-to-arabic/romanToArabic.spec.js "https://github.com/DaveMBush/JavaScriptKatas/blob/Step-5/tests/roman-to-arabic/romanToArabic.spec.js") And the code to implement it. [https://github.com/DaveMBush/JavaScriptKatas/blob/Step-5/katas/roman-to-arabic/romanToArabic.js](//github.com/DaveMBush/JavaScriptKatas/blob/Step-5/katas/roman-to-arabic/romanToArabic.js "https://github.com/DaveMBush/JavaScriptKatas/blob/Step-5/katas/roman-to-arabic/romanToArabic.js")

Test 6
------

So, before we move on to actually computing the value of the Roman number, we should ask ourselves if there are any other ways a Roman number could be passed in incorrectly. The next one that occurs to me is this.  If you have a number that contains a ‘digit’ from the minusOneTable, the character that is used to subtract should never follow the digit.  For example, if “IV” shows up, there shouldn’t not be an “I” immediately after it.  That is, we shouldn’t see “IVI” anywhere in our string.  So let’s add that to our tests. [https://github.com/DaveMBush/JavaScriptKatas/blob/Step-6/tests/roman-to-arabic/romanToArabic.spec.js](//github.com/DaveMBush/JavaScriptKatas/blob/Step-6/tests/roman-to-arabic/romanToArabic.spec.js "https://github.com/DaveMBush/JavaScriptKatas/blob/Step-6/tests/roman-to-arabic/romanToArabic.spec.js") And the code to implement it [https://github.com/DaveMBush/JavaScriptKatas/blob/Step-6/katas/roman-to-arabic/romanToArabic.js](//github.com/DaveMBush/JavaScriptKatas/blob/Step-6/katas/roman-to-arabic/romanToArabic.js "https://github.com/DaveMBush/JavaScriptKatas/blob/Step-6/katas/roman-to-arabic/romanToArabic.js")

Step 7
------

There is one more possible problem we could encounter.  What if someone passes in a character that isn’t a valid Roman number character.  We need to make sure that the only characters that show up are Roman number characters. Since testing for all of the characters isn’t practical, we are just going to test for a few and assume that if this were a real business problem we’d go to the trouble of testing more.  But, the basic test is going to look the same.  Toss in bad characters in an otherwise valid string and make sure we throw an error. [https://github.com/DaveMBush/JavaScriptKatas/blob/Step-7/tests/roman-to-arabic/romanToArabic.spec.js](//github.com/DaveMBush/JavaScriptKatas/blob/Step-7/tests/roman-to-arabic/romanToArabic.spec.js "https://github.com/DaveMBush/JavaScriptKatas/blob/Step-7/tests/roman-to-arabic/romanToArabic.spec.js") And the solution, which is pretty simple. [https://github.com/DaveMBush/JavaScriptKatas/blob/Step-7/katas/roman-to-arabic/romanToArabic.js](//github.com/DaveMBush/JavaScriptKatas/blob/Step-7/katas/roman-to-arabic/romanToArabic.js "https://github.com/DaveMBush/JavaScriptKatas/blob/Step-7/katas/roman-to-arabic/romanToArabic.js")

Step 8
------

OK. I think that gets all of the validations except for ensuring that the numbers show up in numerical order.  To make this work, we are going to need to work through the array and assign each digit a value.  The test we need to write is going to put values in the wrong order. Our test will put the next value up after the value. [https://github.com/DaveMBush/JavaScriptKatas/blob/Step-8/tests/roman-to-arabic/romanToArabic.spec.js](//github.com/DaveMBush/JavaScriptKatas/blob/Step-8/tests/roman-to-arabic/romanToArabic.spec.js "https://github.com/DaveMBush/JavaScriptKatas/blob/Step-8/tests/roman-to-arabic/romanToArabic.spec.js") And the solution will put them in order and add them along the way. [https://github.com/DaveMBush/JavaScriptKatas/blob/Step-8/katas/roman-to-arabic/romanToArabic.js](//github.com/DaveMBush/JavaScriptKatas/blob/Step-8/katas/roman-to-arabic/romanToArabic.js "https://github.com/DaveMBush/JavaScriptKatas/blob/Step-8/katas/roman-to-arabic/romanToArabic.js")

Step 9
------

And finally we add some test to verify that we get the right result. [https://github.com/DaveMBush/JavaScriptKatas/blob/Step-9/tests/roman-to-arabic/romanToArabic.spec.js](//github.com/DaveMBush/JavaScriptKatas/blob/Step-9/tests/roman-to-arabic/romanToArabic.spec.js "https://github.com/DaveMBush/JavaScriptKatas/blob/Step-9/tests/roman-to-arabic/romanToArabic.spec.js")

Review
------

What?! I thought we were done? Well, we are and we aren’t.  We have code that works.  But is it the best code we can write? Here is what I like about this code:

1.  It is easy to reason about.
2.  It allows me to add additional roman numbers by expanding my tables.  No additional code would be needed. (And while it is hard to represent using Arabic letters, there are additional symbols.)
3.  It works reasonably fast.

But, it does seem to me that we might make it a bit more efficient without sacrificing these advantages too much.  And the great news is, since we have our test in place, we can refactor with impunity.  No worries about breaking something because we will know as soon as we have and we can revert back to the code that was working.

Step 12
-------

One pretty simple change we can make is that our error message is scattered throughout our code.  Let’s make that a variable and just throw the variable. The other thing I wonder about is how much of our validations can be combined? One check we might safely eliminate at this point is the check to make sure the minus values only show up once.  If any of them were to show up more than once, they would either show up one after the other, which we still can check for, or they would be in the wrong value order which our final check will catch.  So, let’s eliminate that code as well and re-run our tests to make sure nothing broke. And now that we’ve eliminated that check, we can create one big string for checking conditions like “IVI” into one string and check it once instead of creating a new RegExp object multiple times. Here is the code we have so far: [https://github.com/DaveMBush/JavaScriptKatas/blob/Step-10/katas/roman-to-arabic/romanToArabic.js](//github.com/DaveMBush/JavaScriptKatas/blob/Step-10/katas/roman-to-arabic/romanToArabic.js "https://github.com/DaveMBush/JavaScriptKatas/blob/Step-10/katas/roman-to-arabic/romanToArabic.js")

Step 11
-------

Now that we have our IVI check down to one string, it occurs to me that we can combine this check with our invalid character check.  So, let’s do that next. [https://github.com/DaveMBush/JavaScriptKatas/blob/Step-11/katas/roman-to-arabic/romanToArabic.js](//github.com/DaveMBush/JavaScriptKatas/blob/Step-11/katas/roman-to-arabic/romanToArabic.js "https://github.com/DaveMBush/JavaScriptKatas/blob/Step-11/katas/roman-to-arabic/romanToArabic.js")

Step 12
-------

And the last place we can optimize is our three digit check.  But instead of limiting it to valid Roman numbers, let’s just check for any sequence of characters that repeats 4 times or more. And once we know that is working, we can combine it with our other Regular Expressions. [https://github.com/DaveMBush/JavaScriptKatas/blob/Step-12/katas/roman-to-arabic/romanToArabic.js](//github.com/DaveMBush/JavaScriptKatas/blob/Step-12/katas/roman-to-arabic/romanToArabic.js "https://github.com/DaveMBush/JavaScriptKatas/blob/Step-12/katas/roman-to-arabic/romanToArabic.js")

Step 13
-------

We’ve cleaned up about all of the logic we can without hard coding the values.  But I would like to combine the lookup tables next.  I don’t like having three tables.  Let’s see if we can pull those into one table. The first step to doing this is combining the minusOneTable and baseTable into one table.  To differentiate in the code that is using those tables, we’ll just look at the length of the property. In the process of doing this, we notice that it would also make sense to combine the substitution table.  And since the values we’ve assigned to the table aren’t even being used at this point, we’ll just use the substitutions instead of the values. And if we make the value of each element in the baseTable an array, we could combine the values into that table as well.  But that would over complicate our lookup logic when we compute the value.  So, I think we’ll just leave that as it is. [https://github.com/DaveMBush/JavaScriptKatas/blob/Step-13/katas/roman-to-arabic/romanToArabic.js](//github.com/DaveMBush/JavaScriptKatas/blob/Step-13/katas/roman-to-arabic/romanToArabic.js "https://github.com/DaveMBush/JavaScriptKatas/blob/Step-13/katas/roman-to-arabic/romanToArabic.js")

Step 14
-------

Finally, I think we want to consolidate the loops into one loop. [https://github.com/DaveMBush/JavaScriptKatas/blob/Step-14/katas/roman-to-arabic/romanToArabic.js](//github.com/DaveMBush/JavaScriptKatas/blob/Step-14/katas/roman-to-arabic/romanToArabic.js)

Review
------

There are probably a few other optimizations that could be made here.  But I’m afraid that each would sacrifice either the flexibility.  That is, we could hard code the regular expression validations, which would significantly reduce the number of lines of code.  Or we would sacrifice the readability.  Neither of which I am willing to do. If you are only interested in the result, you can find the finished project at. [https://github.com/DaveMBush/JavaScriptKatas](//github.com/DaveMBush/JavaScriptKatas "https://github.com/DaveMBush/JavaScriptKatas")
