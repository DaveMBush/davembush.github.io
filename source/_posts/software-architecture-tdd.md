---
title: Software Architecture without Test Driven Development is DANGEROUS!
tags:
  - dependency injection
  - MVC
  - mvp
  - mvvm
  - software architecture
  - tdd
url: 2851.html
id: 2851
categories:
  - Software Architecture
  - TDD
date: 2015-01-29 07:00:00
---

![TDD Impacts Software Architecture](/uploads/2015/01/TddImpactsSoftwareArchitecture.png "TddImpactsSoftwareArchitecture")I’ve had two incidents recently that have shown me how TDD impacts Software Architecture.  Both of these are with code I’m working on.

What Software Architecture Might Do
-----------------------------------

Software architecture might specify how is put together at a very high level.  For example, software architecture might specify that we use a three tiered approach or an n-tiered approach.  This approach places our view code is at one level, our business rules are at another level, and our data access at yet a third level. Software architecture might specify that we use MVC where our business rules are in the model, and a controller communicates between the view and the model to get data between the two. It might specify MVVM. This would have the view model take the place of the controller and manage the state information for the view. Software architecture might also tell us we should use MVP, giving the presenter the role of the controller and managing state information for the view and communicating with the business rules. But none of these patterns tell us how to write maintainable code.  They only tell us about the general software architecture.  This is like having a sketch of a house without a wiring or plumbing plan.

When the view gets in the way
-----------------------------

So, if you’ve been following this blog for a while, you may remember that I’m working with EXTjs.  Specifically, I’m working with EXTjs 4.2.  This has what Sencha calls an MVC architecture.  The problem is, what they refer to as the “Model” we would all recognize as a “Record” in a table, and their Controller is tightly coupled to their View.  That is, they call this MVC, but no one who understands what MVC is supposed to look like as a design pattern would recognize Sencha's MVC as the real MVC design pattern. This makes the code incredibly hard to test.  The tendency is to write code that is highly dependent on the view.  The view is dependent on the DOM.  Rendering the view takes quite a bit of time.  So any test of your business rules end up taking an incredibly long time to test because they ultimately cause DOM manipulation to occur. It isn’t until you decide to borrow a bit of architecture from Angular that you realize that your business rules should be separate classes.  Angular has “Service Classes.” My Service classes are built specifically so they do not rely on anything else. By doing this, I was able to get two thirds of my code under test that run in about a second.  Prior to this, they took a half an hour. My next task was to get the view and my logic for enabling and disabling controls on the view more loosely coupled.  This was a bit more difficult because enabling and disabling controls is, naturally, a view thing. But again, taking a page from another framework, this time Knockout and Angular, I created a ViewModel.  My ViewModel holds the state of my view separate from the actual view.  When the state changes, it fires an event that actually changes the view, but this will allow me to test my enable/disable logic, along with other code in my system, without ever instantiating the view.  Under test, the events will fire and nothing will happen.

Avoiding Dependencies
---------------------

Now the structure  of my code looks something like the following: View – ViewModel – EnableDisableController – EnableDisableService I could have put the EnableDisableService code in the EnableDisableController, and many people would, but what I’ve found is that if I do that, it would be nearly impossible to UNIT test my Enable/Disable logic.  Why?  Because I would be creating all of the objects I needed for the logic in the same class the logic is in. By breaking the logic code into it’s own class that takes the ViewModel as a construction parameter, I can create my own ViewModel that looks exactly like what I need it to look like so that I can test the logic with entirely known values. These are just two of the ways that code architecture is impacted by Test Driven Development.  I’m sure there are others. How have you found TDD helps improve your software architecture?  Leave me a comment below.  

Other Places Talking About TDD and Software Architecture
--------------------------------------------------------

*   [Applying Test-Driven Development to Software Architecture to Keep Your Team on Target](//www.informit.com/articles/article.aspx?p=2174264)
*   [How Does Software Architecture Fit With TDD](//sarahtaraporewalla.com/design/testing/how-does-architecture-fit-with-tdd/)
    
*   [TDD as an Approach for Software Design](//effectivesoftwaredesign.com/2014/07/24/iasa-israel-meeting-dror-helper-on-tdd-as-an-approach-for-software-design/)
