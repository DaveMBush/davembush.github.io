---
title: NUnit 2 vs NUnit 3.  What you need to know.
tags:
  - nunit
  - tdd
  - testing
url: 3295.html
id: 3295
categories:
  - Testing
date: 2015-12-03 08:30:00
---

NUnit 3 recently released and if you’ve picked it up you’ve probably already found that there are several changes between version 3 and version 2. If you haven’t, here are some highlights: ![image](/uploads/2015/11/image3.png "image")

Parallel Tests
--------------

This is probably the most anticipated change.  We can finally run multiple tests at the same time. In NUnit 3.0, we finally have the ability to run multiple tests at the same time by using the `Parallelizable` Attribute in combination with the `LevelOfParallelism` attribute. By default, all of our tests run sequentially.  But if you've written your tests correctly, you should be able to run them all at the same time.  To enable this feature, you'll need to first specify how many tests you can run at the same time by adding the `LevelOfParallelism` attribute to your assembly. In your `properties/AssemblyInfo.cs` file add the following code:

\[assembly: LevelOfParallelism(8)\]

To make a test run at the same time as other tests, add the `Parallelizable` attribute to either the namespace, or class.  As of NUnit 3.0, running tests within a fixture in parallel is not yet supported. The Parallelizable attribute takes one optional parameter named `ParallelScope`.  This option allows you to specify what level of code can run in parallel.

*   None: No tests can run in parallel
*   Self: This level can run in parallel with other similar levels
*   Children: Test under this level can run in parallel.
*   Fixtures: Only fixtures under this level can run in parallel.

Attributes applied to nested namespaces or classes override attributes applied to namespaces they are within.

Test Runner
-----------

Another big change is the test runner.  By default, the test runner is just a regular console app.  I don’t know about you, but I miss the tree view.  But it is what it is. I would suggest that you invest in ReSharper just to get the test runner that it has.  If you can’t do that, use the MS Test Adapter from NUnit.org to run your tests in Visual Studio using the MS Test Runner.

ExpectedException Attribute
---------------------------

In NUnit 2, we had the option of using the `ExpectedException` attribute on a test that was supposed to throw an exception or we could use the `Assert.That` syntax that we've already documented.  In NUnit 3, `ExpectedException` is no longer available.

TestFixtureSetUp/TestFixtureTearDown
------------------------------------

In NUnit 2, when we wanted to have a method that only ran once for a test class as part of setup or teardown, we would use these two attributes.  We would also use these in combination with the `SetupFixture` attribute to run methods once at the namespace level. In NUnit 3, these have been replaced with `OneTimeSetUp` and `OneTimeTearDown` in both cases. While `TestFixtureSetup` and `TestFixtureTearDown` will continue to work as part of a `TestFixture`, you should move to the new attributes as soon as possible.

TestCaseSource
--------------

If you specify the property to use for your tests data using a string, that property now HAS to be static.

ValueSource
-----------

Similarly, this now only uses a static property when specified as a string.

Asserts
-------

Several assert syntaxes have changed.  Once again, check the documentation. For a full list of [breaking changes](//github.com/nunit/nunit/wiki/Breaking-Changes), check out the [NUnit 3 documentation](//github.com/nunit/nunit/wiki). What significant changes have you noticed? Let me know in the comments.
