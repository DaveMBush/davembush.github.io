---
title: Are You Doing Angular Right?
tags:
  - angular.js
  - best practices
  - javascript
url: 3271.html
id: 3271
categories:
  - Javascript
date: 2015-11-05 07:30:00
---

I’ve written that [I’m using Angular](/tags/angular-js/) to write a couple applications before.  One at my main contract and a couple side projects.  I know I’m kind of late to the game, but one of my frustrations with the documentation around Angular is that very little of the sample code that you can find on the Internet shows the sample using anything close to a best practice.  That’s the danger of writing about something you are too familiar with. So, in this post, I’d like to cover a few best practices that I’ve discovered, or implemented in my own code, and explain in a bit more detail what is going on inside the controller.  I concentrate on the controller because this is a place that will be used the most.  Once you understand it, the rest of what you need to know will trickle down to services, factories, and directives.

![image](/uploads/2015/10/image4.png "image")
--------------------------------------------------------------------------

IIFEs
-----

The first thing you’ll almost never see in the demos, but you might pick up on eventually, is that you should always write your angular code inside of and IIFE.  What’s an IIFE you ask?  IIFE stands for Immediately Invoked Function Expression.  The most common looks like this:

(function(){
    // your code goes here
})();

Why do we do this?  So that we can declare variables that are “global” to everything inside of the IIFE without polluting the global namespace.  If you follow the rest of the tips in this article, you shouldn’t end up writing any kind of global variables.  But, there is another good reason for using an IIFE.  You can pass parameters into it.  The most common implementation I use is that I pass any global variables I need for the code inside as parameters.  This keeps JSHint happy without having to specify global variables as comments or putting them in a configuration file.  Since “angular”:

(function(angular){
 // your code goes here 
})(window.angular);

 

Single Responsibility Applies to Files
--------------------------------------

Another big problem I see with a lot of samples, is that they mix controllers and directives and modules declarations all together.  At one level I get it, why separate them when it is just a small demo.  On the other hand, doing inadvertently promotes bad code to the masses. How much code have you seen that looks like:

var app = angular.module('app', \[
    // Angular modules
    // Custom modules
    // 3rd Party Modules
    \]);
app.config(\['
    function () {
        // code here
    });
angular.module('Controllers').controller('Controller',\[ function () {
   // code here
    }\]);
angular.module('Controllers').controller('controllerName',
    \['$scope', function ($scope)
    {
       // Code here
    }\]);

When they should specify what should be in a separate file.  Maybe by wrapping them in IIFEs.

// top level app.js file
(function(angular){
angular.module('app', \[
    // Angular modules
    // Custom modules
    // 3rd Party Modules
    \]);
})(window.angular);

// config.js that lives in a file parallel to app.js
(function(angular){
angular.module('app').config(\[' function () { // code here }); })(window.angular); // controllers holder that lives parallel to app.js (function(angular){ angular.module('Controllers').controller('Controller',\[ function () { // code here }\]); })(window.angular); // controller that lives parallel to the view it is associated with (function(angular){ angular.module('Controllers').controller('controllerName', \['$scope', function ($scope) { // Code here }\]); })(window.angular);

Namespace Everything
--------------------

As I’ve already hinted at in the code above, I place my controllers and the views they are attached to in separate directories based on the route or state that I will use to get to them.  I use states, so to be clear the state does not necessarily represent the url that I will be using to access the view.  This works out pretty nicely because for every state, there is a directory and for every sub state, there is a sub directory.  Each directory generally has two files.  controller.js and view.html.  When I specify the name of the controller, I use dot notation to represent where it is in the directory structure.  Similarly, when I specify the state in app.config(), I use the same dot notation.  So if my state were ‘edit.contact’ you should expect to see a directory structure of ‘edit/contact’ with the two files inside of it.

ControllerAs
------------

I’ve only seen this explained in one place ([PluralSight of course](/pluralsight)) where this is explained in a way that I could grasp. Here’s the deal.  What you first need to understand is how the code you write in angular relates to the code you would write in JavaScript without the framework.  (BTW, one complaint I’ve heard from hiring managers is that while they can find people who can write Angular, they can’t find people who can write Angular AND understand JavaScript.  So much is hidden from us by using Angular.  If you want to be awesome at Angular, be awesome at JavaScript.) So, let’s sketch out a basic controller.

(function ()
{
    'use strict';
    angular.module('Controllers').controller('edit.contacts',
        \['$scope', function ($scope)
    {
        // Your controller code here
    }\]);

})();

What is happening here?  Well what you’ve essentially done is create an anonymous constructor for your controller.

Let’s rearrange some code so that you can see this more clearly.

(function ()
{
    'use strict';

    editContacts.$inject = \['$scope'\];
    function editContacts($scope)
    {
        // your code here
    };

    angular.module('Controllers').controller('edit.contacts', editContacts);
})();

So, now the code looks like regular JavaScript.  And if I told you that editContacts was the object constructor, the next thing that you might expect is that you could then write code that looked something like this:

(function ()
{
    'use strict';

    editContacts.$inject = \['$scope'\];
    function editContacts($scope)
    {
        var self = this;
        function someMethod(){
            self.variableName = 'foo';
        };

        self.someMethod = someMethod;
        // your code here
    };

    angular.module('Controllers').controller('edit.contacts', editContacts);
})();

This is the way I like to write JavaScript classes, but you can use any method you prefer.  It is just plain old JavaScript at this point. The other piece that might need some explaining is the ‘editContacts.$inject’ thing.  This is just an alternate way of specifying the services that need to be injected into your constructor.  I haven’t decided if I like using this way, or if I like having them specified in the controller definition at the bottom.  By leaving the injection at the bottom, I am left with code that looks mostly like plain old JavaScript.  By leaving them at the top, I can see that what I’m injecting matches the parameters I am expecting.  Of course, if you are willing to throw some external tools into the mix, you can have that line created for you automatically at build time. So, under the hood, what Angular is doing for you is looking for the controller by the name that you gave it and calling the function associated with it using the ‘new’ keyword.  If you want some of your functions and methods exposed to the view, you need to attach them to the $scope, which is why we need to inject it as a parameter to our constructor.  But, if you use the ControllerAs keyword, it assigns the object to a variable the the name you specified as the ‘As’ and attaches it to $scope.  What this means is that you almost never have to pass in the $scope object to your constructor and all of your public methods and fields are available to the view for free using the syntax ‘asName.variableName’.

Using Controllers in Directives
-------------------------------

The final piece I want to talk about today gets back to my discussion about demos that don’t use good coding practices.  Most directive demos that use controllers write the controller inline.  But you don’t have to do it this way, and I would recommend that you don’t.  Controllers should be in their own file.  Separation of concerns and Single Responsibility.  You define  your controller just like you would any other controller in the separate file.  I define them using the controller function hanging off the directive object

directive('directiveName')
    .controller(controllerDefinitionHere);

And then use the name of the directive in the controller property in my directive code.
