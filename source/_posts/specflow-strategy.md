---
title: SpecFlow Strategy
tags:
  - programming
  - specflow
  - tdd
  - test driven development
url: 2803.html
id: 2803
categories:
  - SpecFlow
date: 2015-01-01 07:00:00
---

A while ago I wrote an article [explaining what SpecFlow is, and why you might want to use it](/what-is-specflow/).  I’ve been using it for several months now and I’ve recognized several patterns that have emerged in my usage that I wish I had known when I started, so I thought I’d share them with you today.

<!-- more -->

Small Features
--------------

If you are used to BDD or even unit testing, this will be obvious.  But if you are new to all of this, the temptation is going to be to put all of your test in one feature.  While this works in the strictest sense of the word, and you are at least getting test in place, you will find that finding a specific test later on is going to be rather awkward.

A feature file should really only have a few scenarios that are testing a particular feature in the system.  This is why, at the top of your feature file, you are stating what exactly the feature is:

As an administrator
I need to be able to delete existing users
So they cannot access the system any more

Which we would probably put in the “DeleteUser.feature” file. Now that we’ve specified the feature, we need one or more scenarios for this. Each scenario is going to test the various ways you might be able to delete a user.  Maybe you can check boxes and delete them all at once or you can click one button per user and delete them one at a time.  Each of those is a scenario.  So your scenarios might look like this:

``` cucumber
Scenario: Delete one user at a time
    Given I have 10 users in the system
    And I have logged in as an administrator
    And I have navigated to the user admin page
    When I click the first delete user button
    Then I only have 9 users in the system
    And I remain on the user admin page

Scenario: Delete one users at a time with checkbox
    Given I have 10 users in the system
    And I have logged in as an administrator
    And I have navigated to the user admin page
    When I check the checkbox of the first user
    And I click the delete checked users button
    Then I only have 9 users in the system
    And I remain on the user admin page

Scenario: multiple users at a time using the checkbox
    etc ...
```

What about sad path test?  You’ll need to determine if those are another feature or if they are somehow part of the feature you are working on.  For this test, I can’t think of a sad path test for deleting users that is generally applicable.  Most of the time, if you have something that should occur as part of deleting a user, you should be doing that as part of the delete process.

Don’t Repeat Yourself
---------------------

The next place you are likely to make a mistake as you learn how to use SpecFlow is that you’ll start out thinking of the feature file, the steps implementation for the features and any setup or teardown of the scenarios as all one unit.  As a result of this, you’ll tend to implement the steps and the setup and teardown (what SpecFlow implements as Before and After) all in one file.

The problem with this is that you’ll end up with a lot of duplicate code in your tests.

Now, let’s say each of your features have similar setup code you need to implement.  In the case of our user administration example that we started out with, you’ll have code you need to implement for adding a user, deleting a user, adding roles to a user, searching for a user, and probably more.

For each of those, your Before code is all going to look the same.  So, what I would recommend that you do is create a separate file with your Before and After code in it, nothing more.  You will associate this file with your feature files by associating a category with it and using the category in your feature files.  Now for each feature, the same Before and After code will run.

The other issue you are going to have is that you are going to have a lot of the same steps between the feature files.  However, my experience has been that there will be a lot more common features between the code I write for a particular screen than there will be for the Before and After code I write.  For example, I might right Before code for various roles

``` cucumber
As a regular user
When I visit the user administration screen
I should not be able to view any user other than myself.
```

That will require different setup code from “As an administrator” but the steps to implement the scenarios will probably be very similar.

Therefore, you should have a step file that holds all the steps for a screen full of logic in a file separate from your Before and After logic.
