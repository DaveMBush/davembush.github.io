---
title: SpecFlow Tutorial
tags:
  - bdd
  - specflow
  - testing
url: 3112.html
id: 3112
categories:
  - Testing
date: 2015-06-04 06:00:00
---

![SpecFlow Tutorial Session](/uploads/2015/05/18Tr.png "18Tr")A couple of weeks ago, I wrote a post describing [how to setup SpecFlow](/setting-up-specflow/) in which I promised to continue with how to actually use it once you have it installed.  What follows is a SpecFlow Tutorial of the tips and tricks I wish I’d known when I was starting out. 

Controlling Associations between Features and Steps
---------------------------------------------------

With what you’ve learned so far, you might be able to successfully put together a set of tests for a particular page that you want to test. But as soon as you start working on the second page, you will probably discover that you have steps that seem to conflict with each other. It is at this point you will find that the ability to associate features with step files becomes critical. Fortunately, this is relatively easy. In your feature file, you will specify a category using the @category syntax and within your step file, you will use the category attribute within the step file at the class level to tie the two together. In your feature file:

@stepFileOne
Feature: SpecFlowFeature1
    In order to avoid silly mistakes
    As a math idiot
    I want to be told the sum of two numbers

And in your step file:

namespace SpecFlowDemos
{
    \[Binding\]
    \[Category("stepFileOne")\]
    public sealed class StepFileOne
    { …
    }
}

Note, if you put steps in a step file that doesn’t have a category associated with it or you create a feature file that doesn’t have a category assigned to it, you’ll see all of the steps that are available. So you will want to have categories applied to everything. If you need to, you can apply categories to specific scenarios in your feature file and specific steps in your step file to gain even more control.

Don’t Repeat Yourself (DRY)
---------------------------

### Background section

It is common to have a set of steps that repeat for each scenario. Much like a setup section in NUnit or MSTest. One of the ways you can handle this is by creating a background section.

Background: 
    Given I have entered 50 into the calculator
    And I have entered 70 into the calculator

Scenario: Add two numbers
    When I press add
    Then the result should be 120 on the screen

The background section runs for each scenario in the feature file.

### Running Steps from another Step

You could also create a step that combines all of the common steps together and then have that step run all of the steps.

Scenario: Add two numbers
    Given the calculator has been setup
    When I press add
    Then the result should be 120 on the screen

  Then in your Step file, you’ll need to change your Step class to inherit from Step

public sealed class StepFileOne : Steps
{
 …
}

And then create your step to call the other steps like this:

\[Given(@"the calculator has been setup")\]
public void GivenTheCalculatorHasBeenSetup()
{
    Given("I have entered 50 into the calculator");
    Given("I have entered 70 into the calculator");
}

Yes, you could call the methods directly, but only if the methods are in the same class.  If you have step methods that span multiple classes, you’ll want to use this method so that you can maintain the context of the test you are running.

### Before/After Attributes

Finally, you may have setup steps that really have nothing to do with the actual test. In the case of testing web pages or other applications, you’ll probably have to load the page or start up the application and wait for the page to be ready to use prior to doing anything else. And once you have completed your test, you’ll want to do some clean up. At the very least, you’ll want to close the web page. For this, SpecFlow provides the Before attribute and the After attribute. Just like the step definitions, the methods attributed with Before and After will run based on the categories they are associated with.

\[Before\]
public void BeforeScenario(“categoryToAssociateWith”)
{
    //TODO: implement logic that has to 
    // run before executing each scenario
}
 
\[After\]
public void AfterScenario(“categoryToAssociateWith”)
{
    //TODO: implement logic that has to 
    //run after executing each scenario
}

Other before/after attributes that you can use (from [//www.specflow.org/documentation/Hooks/](//www.specflow.org/documentation/Hooks/))

**Attribute**

**Tag filtering**

**Description**

\[BeforeTestRun\] \[AfterTestRun\]

-

automation logic that has to run before/after the entire test run_Note: As the most of the unit test runners do not provide a hook for executing logic after the test execution has been finished, the \[AfterTestRun\] event is triggered by the test assembly unload event. The exact timing and thread of this execution might be different for each test runner._The method it is applied to must be static.

\[BeforeFeature\] \[AfterFeature\]

+

automation logic that has to run before/after executing each featureThe method it is applied to must be static.

\[BeforeScenario\] or \[Before\] \[AfterScenario\] or \[After\]

+

automation logic that has to run before/after executing each scenario or scenario outline exampleThe short attribute names are available from v1.8.

\[BeforeScenarioBlock\] \[AfterScenarioBlock\]

+

automation logic that has to run before/after executing each scenario block (e.g. between the "givens" and the "whens")

\[BeforeStep\] \[AfterStep\]

+

automation logic that has to run before/after executing each scenario step

You can annotate a single method with multiple attributes.

Sharing Data Between Step and Binding Files
-------------------------------------------

Once you’ve setup your tests with the `Before` and `After` methods, you will probably need to save the information in a location where you can get to it while you are running the tests. You might be tempted to place this information in a member variable or a property but, if you’ve done as I’ve suggested, and placed your `Before` and `After` methods in separate files from your steps, you’ll see that this won’t make a lot of sense. Instead, what I recommend that you do is that you save the information into the `ScenarioContext` object using this syntax.

ScenarioContext.Current\["page"\] = someValue;

Then, later on in your testing process, you can retrieve the value using:

var x = (RealObjectType)ScenarioContext.Current\["page"\])

where “RealObjectType” is a class name that represents the type of the object that you originally stored. I typically use the Before and After scenarios to open and close the page I want to test while using Selenium. Without going into a whole lot of detail about how this works, my Before and After routine looks something like this:

private ISpmInitial Page
{
    get { return (ISpmInitial)(ScenarioContext.Current\["page"\]); }
    set { ScenarioContext.Current\["page"\] = value; }
}
 
\[Before\]
public void Before()
{
    const string id = "10039219";
    var pages = new ConcurrentStack<ISpmInitial>();
    Parallel.Invoke(
        () =\> pages.Push(SpmPageFactory.GetPageModel("FireFoxGrid", id))
        ,
        () =\> pages.Push(SpmPageFactory.GetPageModel("IE11Grid", id))
    );
    var pagesArray = pages.ToArray();
    Page = new ParallelPage<ISpmInitial>(pagesArray).Cast();
}
 
\[After\]
public void After()
{
    if (Page == null) return;
    Page.Dispose();
    Page = null;
}

This code is using the Selenium parallelization code I’ve already talked about in my article, [Running Selenium In Parallel With Any .NET Unit Testing Tool](/running-selenium-in-parallel-with-any-net-unit-testing-tool/). The `Before` method creates my page object for the browsers I’m using. Once I have the page object, I store it into my private `Page` property which uses the `ScenarioContext` as the backing store. Then, in the step files that need access to this same page object, I create a similar `Page` property so that they can access the page object from the steps.

Parameterized Tests
-------------------

It is quite common to have tests for multiple scenarios that essentially look the same except for the parameters change. Instead of using copy/paste/modify, you can create parameterized test by using the Scenario Outline directive.

Scenario Outline: Uses parameters
    Given I have entered <n1> into the calculator
    And I have entered <n2> into the calculator
    When I press <operation>
    Then the result should be <result> on the screen

    Examples: 
    | n1 | n2 | operation | result |
    | 1  | 2  | add       | 3      |
    | 5  | 3  | subtract  | 2      |
    | 5  | -3 | add       | 2      |

Keep in mind that the step that gets run is based on the merged result. So you could have multiple steps that implement any particular line in your scenario outline.

Alternate ways of structuring your test
---------------------------------------

### Data Tables

Scenario: Data Table Example
    Given I've Entered The Following Information
    | Field     | Value                   |
    | FirstName | "Dave"                  |
    | LastName  | "Bush"                  |
    | email     | "davembush@dmbcllc.com" |

  The first column is the names of the fields. The second column is the values. To pull the information out in the step, create, or use an existing class that has the same field names and write code similar to the following in your step.

class Person
{
    string FirstName { get; set; }
    string LastName { get; set; }
    string email { get; set; }
}
 
\[Given(@"I've Entered The Following Information")\]
public void GivenIVeEnteredTheFollowingInformation(Table table)
{
    var person = table.CreateInstance<Person>();
    // Do something with person here
}

You can use a horizontal layout like this:

Scenario: Data Table Example
    Given I've Entered The Following Information
    | FirstName | LastName | email                   |
    | "Dave"    | "Bush"   | "davembush@dmbcllc.com" |
    | "John"    | "Doe"    | "jdoe@gmail.com"        |

In this case you use the `CreateSet` call to create a list of Person objects.

\[Given(@"I've Entered The Following Information")\]
public void GivenIVeEnteredTheFollowingInformation
    (Table table)
{
    var people = table.CreateSet<Person>();
    // Do something with people here
}

Finally, you may have the need to enter parameter data that is longer than will reasonably fit on one line or that needs to be on multiple lines. To do that, you delimit the beginning and end of the multi-line section with three double quotes.

\[Given(@"I have lots of text")\]
public void GivenIHaveLotsOfText(string multilineText)
{
    ScenarioContext.Current.Pending();
}

The multi-line text will be passed as the last parameter in your step method. There are probably some features I missed here.  But this is already more than what I use on a day to day basis in my own SpecFlow testing scenarios.
