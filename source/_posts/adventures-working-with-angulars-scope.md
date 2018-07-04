---
title: Adventures Working With Angular’s $scope
tags:
  - angular
  - javascript
  - scope
url: 3289.html
id: 3289
categories:
  - Javascript
date: 2015-11-26 08:30:00
---

Every week when I write, I try to think back on the past week and think, “What have I learned that might be useful to others.”  Most weeks that is a pretty easy question to answer because I get most of my pleasure from learning new stuff.  But this week was different. When I sat down to write today, I couldn’t come up with a subject that couldn’t be covered with a sentence.  More of a tweet than a blog post.  It was so bad that I decided to go run the errands that are on my list and come back to it once I got home. Evidently, that was a good move because I think I have something that will be genuinely useful.  Although I will admit that if you’ve been working with Angular for very long at all, you may have already learned what I’m about to explain. ![image](/uploads/2015/11/image2.png "image") 

The Basics
----------

To make sure we are all on the same page, I want to cover the basics first.  Every controller has a $scope variable associated with it.  $scopes can be nested within each other, which allows us to either have child scopes add information to parent scopes using prototypical inheritance, or the child $scope can be isolated so that it can’t see the parent $scope up the nesting chain.  In either case, you can gain access to the parent $scope by using the $parent variable. What is important to realize about $scope is that it is attached to the element’s this pointer.  Every controller you create is its own object and $scope is just one of many properties that is part of that object. Now, in normal Angular programming, you may only ever create one controller per element.  But if you’ve ever created a directive that gets used on multiple places, or you’ve done anything with repeaters, you know that controllers can be created “under the hood.”  So that the $scope for the repeater item isn’t the same as the $scope for the element that holds the main collection.

$scope and ControllerAs
-----------------------

In version 1.2 of Angular, the ControllerAs syntax was added, this can be added in a number of different ways that I won’t describe here.  That’s old news and there are plenty of places to find that information, including the Angular documentation.  But what isn’t clear at first, is what this feature does for us under the hood. Many people, who haven’t dug in deep under the hood assume that somehow this replaces $scope.  But in fact what it does is that it adds a variable onto $scope.  If you were to use ControllerAs redMonkey, what actually happens under the hood is that you end up with a variable named “redMonkey” that is hanging off of the controller’s $scope variable. In fact, what you end up with is this holding a property “$scope” and $scope holding a property “redMonkey” which is actually pointing to the controller’s this pointer.  Further, it is possible to have elements in your view bind to other variables hanging off of $scope while in the same view, other things are bound to variables hanging off of redMonkeys.

$scope and Singletons
---------------------

I hope at this point you have a relatively clear picture of how $scope works because this is where things start to get interesting.  You see, just about everything else that you create in Angular is a singleton.  That is, only one instance of it exist in the entire application.  So, what happens if you pass $scope into one of these singletons and you use that singleton multiple times on the same page? Well, there’s no telling for sure.  You might get lucky and everything will seem to work, until one day it doesn’t.  In fact, you may never notice that there is a problem if you only ever call it from one controller or directive at a time. But to use singletons effectively, what you need to do is that you will need to pass the scope to each of the functions you need to have use it, unless you can be sure it will only be able to use one scope at a time.  As a general rule, you should never store state in anything that is a singleton. How do I know? Well, this week, I was working on some code that I thought was overriding the control’s controller.  But, when I finally got it all working I found that it only worked some of the time because what I had really overridden was a method in a directive.  Another time this week, I was doing something similar thinking I was overriding a function in the main control’s $scope only to find out that the function I was overloading was in a nested repeater item.

Finding $scope for an element
-----------------------------

If you need help figuring out what $scope is bound to what element in your code, you can use the [Batarang plugin](//chrome.google.com/webstore/detail/angularjs-batarang/ighdmehidhipcmcojjgiloacoafjmpfk?hl=en).  You can also use the following JavaScript code using any developer tool you might want to use.

angular.element(elementSelector).scope()

$scope and Repeaters
--------------------

So, back to one of my issues this week.  What I was working with was a tree control.  The tree was a set of repeaters within repeaters.  The original function that was bound to the click event was bound to the item scope so that each item was bound to its own instance of the onClick method. What I was trying to do was to override the function with a function in a directive I had created that wrapped the tree control and added a search box.  Fortunately, the item template could be changed, but how to get it to call MY click handler instead of the one it called by default? To further complicate matters, the $scope in the directives were isolated so I couldn’t add a new function on the scope I had control over and have the child scope see it. Or could I? The directive I was creating does have access to the scope of the tree control.  So, all I really needed to do was to create a new variable hanging off the tree control’s scope that pointed to the scope I wanted it to see and then in my template I could point to that variable and the function hanging off of it as the thing that should get called on click. Sure enough, that worked.
