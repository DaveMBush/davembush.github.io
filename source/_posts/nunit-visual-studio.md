---
title: NUnit & Visual Studio
tags:
  - nunit
  - test driven development
  - visual studio
url: 2773.html
id: 2773
categories:
  - TDD
  - Testing
date: 2014-12-04 07:00:00
---

Many people starting out with Unit testing get stuck when it comes to using their tools with the Visual Studio environment.  If it isn’t built in, how do we make it work with Visual Studio?  In this article I want to explore the basics of creating a unit test for NUnit and getting it running from Visual Studio.

Basic Structure
---------------

To create a unit test, the first thing you will need to do is to create an assembly with a class file to hold the code. The type of assembly you will need to create for your test to be able to run is an assembly of type "Class Library." I'm going to assume that you don't need the details on how to create that type of project using whatever version of Visual Studio you happen to be using.

Within your new Class Library project, you should find a file named Class1.cs. For the purposes of getting started, you can just leave that file. We'll talk about how to name your class and test later on. For now, all we want to concentrate on is the basics of what is involved in getting a basic test going and what the components of a test class are that you will repeat over and over again as you create unit test for your applications.

Before we add our first line of code, though, you will want to add references to NUnit in your code. The easiest way to do that is by using NuGet. Since I use Visual Studio 2013, all of the references for how to do things will be using Visual Studio (Premium) 2013. So, if you are using that version, you can just follow along. If not, you may need to do some translating.

From the Visual Studio menu, select "Tools" > "NuGet Package Manager" > "Manage NuGet Packages for Solution...". A window will pop up. Search for NUnit and install the package named "NUnit". There are other packages available that we will discus later on.

Alternatively, you could download NUnit directly from the NUnit site ([http://nunit.org](//nunit.org)) and add a reference directly to nunit.framework.dll.

Code
----

Now that you have the DLLs installed and referenced in your project, you'll need to create a test class. For our purposes, we are going to stick with Class1.cs.

There are several things that make a class a test class. First, the class has to be attributed with the \[TestFixture\] attribute. Second, the methods you want to have run the test have to be attributed with the \[Test\] attribute and must be public. So, your minimal test class will look something like:

\[TestFixture\]
public class Class1
{
    \[Test\]
    public void MyFirstTestMethod()
    {
        // test code here
    }
}

Running Tests
-------------

Now that we have a basic test class, go ahead and compile it. We are going to try running the test next. For that we will need a test runner.

There are several ways to run NUnit test. For out purposes, we are going to use the runner that comes with NUnit. But you might also be interested in one of the alternatives. You can get a 30 day trial of [ReSharper by JetBrains](//www.jetbrains.com/resharper/). ReSharper has many features, but the one I want to talk about here is the test runner. Any test you create will be immediately runnable from within Visual Studio by right clicking on an icon to the left of your code. You are presented with a menu of options including debugging your test. Believe me, this is the easiest way to debug NUnit test that I know of.

Another easy way to run NUnit test from within Visual Studio is by installing the MSTest adapter. You can get this from [NuGet](//www.nuget.org/packages/NUnitTestAdapter/).

For the purposes of this article, we are going to use the test runner that comes with NUnit.

So the next thing you'll need to do is go to [NUnit.org](//nunit.org) and download and install the latest version of NUnit if you didn't do that already to get the NUnit DLLs installed. I would suggest using the MSI installer rather than the zip file. All we want to be able to do is to run the test.

Once you've installed NUnit, there should be a menu option "NUnit" that you can click. This should bring up the GUI runner.

![image](/uploads/2014/11/image.png "image")

Use the "File" -> "Open" to navigate to the "bin" directory of your NUnit test DLL project and open the DLL. You should see a screen that looks something like this:

![image](/uploads/2014/11/image1.png "image")

Click the "Run" button to run your test. You should get a green progress bar under the "Run" button and a check box over the icons in the tree on the left indicating that all of the test succeeded.

"Wait?!", you say, "I didn't test anything, how did they succeed?"

You are right. A test succeeds if it doesn't fail. Later on this will impact how we structure our test. So keep this in mind.

One of the nice things about the NUnit GUI runner is that you can keep this up while you work on your tests. By default, the system shadow copies the DLL so that you can compile the DLL in your project. When the NUnit GUI runner sees that the file has changed, it will reload it so that it is always running whatever version you recently compiled. You can change this behavior, if you really feel the need to, by navigating to "Tools" -> "Settings..."

Debugging Tests
---------------

As with all things related to code, eventually you will need to debug your test. To do this using the the tools that come with NUnit, do the following.

Right click on the project in Solution Explorer that represents your test project. From the resulting menu, select "Properties." In the resulting window, select the "Debug" tab from the left-hand side of the window.

![image](/uploads/2014/11/image2.png "image")

You will want to select "Start external program" and point it to the UNnit runner that got installed when you installed NUnit.

Now, whenever you run this project, with or without the debugger, NUnit will start up. Note: there is no reason to pass parameters telling it what DLL you want to run because it will load the last DLL it had up. But, if you wanted to do that, you could pass the location of the DLL as a parameter to the GUI runner. There are other parameters you can us. Check the documentation for the version of NUnit you are using for the specifics.

If you are running .NET 4.x, you'll want to go to the location in your file system where NUnit.exe lives and find the NUnit.exe.config file. Find the startup element (<startup> .... </startup>) and place this line in between the open and close startup tags:

<supportedRuntime version="4.0" />

If you miss this step, you won't be able to debug your 4.0 code. Alternatively, you can just set your project to use .NET 3.5.

So, let's give it a try.

First, put some code in the test method you just created. For our purposes, we'll just put in a console writeline so we have somplace to put a breakpoint.

public void MyFirstTestMethod()
{
    Console.WriteLine("Inside MyFirstTestMethod");
}

.csharpcode, .csharpcode pre { font-size: small; color: black; font-family: consolas, "Courier New", courier, monospace; background-color: #ffffff; /\*white-space: pre;\*/ } .csharpcode pre { margin: 0em; } .csharpcode .rem { color: #008000; } .csharpcode .kwrd { color: #0000ff; } .csharpcode .str { color: #006080; } .csharpcode .op { color: #0000c0; } .csharpcode .preproc { color: #cc6633; } .csharpcode .asp { background-color: #ffff00; } .csharpcode .html { color: #800000; } .csharpcode .attr { color: #ff0000; } .csharpcode .alt { background-color: #f4f4f4; width: 100%; margin: 0em; } .csharpcode .lnum { color: #606060; }

Next set a breakpoint on the Console.WriteLine method and then run your project with the debugger.

Once NUnit loads the DLL, click the "Run" button in NUnit. If everything is setup correctly, you should stop on the breakpoint you set.

Console.WriteLine()
-------------------

You may have noticed that we put several Console.WriteLines() in our code but they aren't displaying anywhere. So, where did they go? How can we see them?

By default the "Text Output" tab displays all of the Console.WriteLine() messages as well as all of the test results. If all you care to see is the test results, you should select the "Errors and Failures" tab. Personally, I prefer to work in the "Text Output" tab and I suggest that you do the same.

Not The Only Way
----------------

This isn’t the only way to get NUnit & Visual Studio working together.  You could also purchase the ReSharper plugin which has many other features.  But one of the ones I use on a regular basis is the NUnit integration.

You could also use the NUnit test Adapter to make NUnit work with the Visual Studio test engine.  But personally, I don’t like the way the test render using that and I’d much rather use the GUI viewer I’ve discussed in this article.  So, if you want to integrate NUnit & Visual Studio for free, what I’ve outline above is the best way to do it.
