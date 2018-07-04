---
title: VB.NET Processing Before WinForm Display
tags:
  - conditional startup
  - vb.net
  - windows forms
  - winforms
url: 1355.html
id: 1355
categories:
  - VB.NET
date: 2009-08-06 06:52:55
---

![arct-075](/uploads/2009/08/arct075.jpg "arct-075")

I woke up this morning to an interesting question.

_“Using VB.net 2008, I want my project to be a Windows Forms Application, but upon startup, I want to check a few files to see if they exist and if they don't I do not want the startup form to load. I just want the program to quit. If you have to start this type of application with a form, how do you keep the form from displaying?”_

If you program in CSharp, you probably already know the answer to this question, or at least you should.  If you don’t, you will when we finish here.  So since I consider this a VB.NET-specific question, I’m going to answer it using VB.NET syntax.

When CSharp runs a Windows Forms application, it writes out the following code in Program.cs (in VS 2008, earlier versions put this in the main form).

\[STAThread\]
static void Main()
{
    Application.EnableVisualStyles();
    Application.
        SetCompatibleTextRenderingDefault(false);
    Application.Run(new Form1());
}

[](//11011.net/software/vspaste)

In VB.NET there is no code that looks like this, because VB.NET writes the code for us behind the scenes.

So to do what you want to do, we need to take over control of the Windows Form application.

Since I’m assuming that you already have the Windows Form application created, the next thing you’ll want to do is to create a module.  You can name it what ever you want, but I’m going to name mine “Main” for purposes of this article.

In your module, create a function called “main” that has the code CSharp would have given us.

    Public Sub main()
        Application.EnableVisualStyles()
        Application.SetCompatibleTextRenderingDefault(False)
        Application.Run(New Form1())
    End Sub 

[](//11011.net/software/vspaste)Now go to your project properties and go to the Application tab.

![image](/uploads/2009/08/image.png "image")

Find the check box that says, “Enable Application Framework” and un-check it.

![image](/uploads/2009/08/image1.png "image")

Then change the startup object to “Sub Main".”

At this point, your application should run as it always has.  To put the checks in that you requested, write that code prior to all the Application… statements that we put in sub main and put an if/then/end if statement around the Application… statements.

[](//11011.net/software/vspaste)

    Public Sub main()
        Dim ChecksWereOk As Boolean = False ' your checks here If ChecksWereOk Then Application.EnableVisualStyles()
            Application. _
                SetCompatibleTextRenderingDefault(False)
            Application.Run(New Form1())
        End If
    End Sub 

[](//11011.net/software/vspaste)And that should do the trick for you.
