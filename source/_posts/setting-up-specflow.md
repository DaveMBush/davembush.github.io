---
title: Setting up SpecFlow
tags:
  - bdd
  - specflow
  - tdd
  - testing
url: 3100.html
id: 3100
categories:
  - SpecFlow
date: 2015-05-21 06:00:00
---

I’ve been asked to train a group of developers in the use of [SpecFlow](//www.specflow.org/) so that they can use it to write Selenium Tests.  So, in an attempt to “kill two birds with one stone” I thought today’s post would cover how to get the SpecFlow environment setup.  Not only will it help me prepare for the training session I will be leading, but it will help me when I need to set this up the next time because it tends to be a bit confusing when you setup a new project.  You’ll see why in a bit.

Installing SpecFlow
-------------------

<!-- more -->

The first thing you’ll want to do is to install the SpecFlow Visual Studio integration.  You do this by navigating to “Tools” –> “Extension and Updates …” ![image](/uploads/2015/05/image.png "image") This will bring up a dialog box where you’ll want to select the “Online” branch ![image](/uploads/2015/05/image1.png "image") You will notice in the upper right corner of this dialog, a search box.  Type in “SpecFlow” to find the SpecFlow extensions.

![image](/uploads/2015/05/image2.png "image") In the image above, I’ve already installed it, so there is a check box.  You’ll want to click the “Download” or “Install” button that displays when it isn’t installed.

Creating a Test Project
-----------------------

Now that you’ve installed the extension, you can create a new test project.  You might expect a SpecFlow template to appear somewhere in the project list.  Sorry, that’s not what you get.  You could create one yourself if you really wanted.  To create a SpecFlow project, all you really need to do is to create a Class Library project.

![image](/uploads/2015/05/image3.png "image")

Adding a Reference
------------------

And now, this is where I always get tripped up for a minute.  You can create a new feature file, but there is a menu item you should see that will allow you to add feature steps to a class file and you won’t see that until you add a reference to SpecFlow.  To do that, you will need to go back to NuGet.  This time use the menu option, “Tools” –> “NuGet Package Manager” –> “Manage NuGet Packages for Solution…”

![image](/uploads/2015/05/image4.png "image")

Once again a dialog will display.  Make sure you are in the “Online” branch on the left and type “SpecFlow” into the search area in the upper right corner of the dialog.

![image](/uploads/2015/05/image5.png "image")

I always use SpecFlow with NUnit, if you install SpecFlow.NUnit it will automatically install the SpecFlow dependencies with it.  If you prefer to use xUnit, you can install that and the SpecFlow dependencies will install automatically.

Create Your First Feature File
------------------------------

Now that you have the environment setup, you can actually use SpecFlow.  To create your first feature file, you can right click on the project and select “Add” –> “New Item…” from the context menu.  Just like you would add a new file to the project any other time.  From the resulting dialog, select “SpecFlow Feature File”, give it an appropriate file name, and click the “Add” button.  When I name my SpecFlow feature files, I try to name them similar to the feature it will be testing.

![image](/uploads/2015/05/image6.png "image")

The resulting feature file will have a sample of how your feature will be setup.  You will want to change this to test whatever it is you are testing.  The next thing you will want to do is to create a Step Definition file.  This is a regular C# file with a Binding attribute added.  The file that SpecFlow will generate for you will match the template they provided in the feature file.  What I want you to do next is the delete all of the methods in the class so that I can show you how to add additional methods.  I’m not going to go to the trouble of showing you the screen shots.  I figure you know how to get to it and the image above shows it listed in the “Add File” dialog.

Next, go back to your feature file and right click on the provided “Given” line, you should see a menu option that says, “Generate Step Definitions”

![image](/uploads/2015/05/image7.png "image")

If you click this, you’ll get a dialog like this:

![image](/uploads/2015/05/image8.png "image")

A couple of things you should notice.  One is the name of the class you are going to create if you click the “Generate” button.  I would advise against using the generate button.  Instead, we are going to use the “Copy methods to clipboard” method.  If you want to try to keep things segmented, you might choose to copy some of the steps to the clipboard and paste them into one file and copy others and paste them into another file.  This list will only list steps that you don’t already have definitions for, so you don’t need to worry about producing duplicate definitions.

If you copy the methods onto the clipboard and paste them into the step file, you should end up with code that is functionally equivalent to what the new file template created for you.

Just A Start
------------

This is enough to get you setup, but there is more to cover, so make sure you subscribe to the mailing list to get notified when I post another article about SpecFlow.
