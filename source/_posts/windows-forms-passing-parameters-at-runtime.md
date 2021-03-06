---
title: Windows Forms - Passing Parameters at Runtime
tags:
  - arguements
  - 'c#'
  - command line
  - main
  - winforms
url: 894.html
id: 894
categories:
  - winforms
date: 2009-03-09 07:37:11
---

![misc_vol4_028](/uploads/2009/03/misc-vol4-028.jpg) I received the following question over the weekend:

> I've made a C# form application and I need to send a report name at runtime. How do I add an incoming parameter to the command line? Such as "crFORM.exe Shipform.rpt"

<!-- more -->

I have to assume the question relates more to how to retrieve the parameter in the code than how to pass it because the example shows how we'd pass it. All WinForms CSharp programs have a static method called Main() that looks something like this:

``` csharp
[STAThread]
static void Main()
{
    Application.EnableVisualStyles();
    Application.SetCompatibleTextRenderingDefault(false);
    Application.Run(new Form1());
}
```

If you are using Visual Studio 2008 to build your applications, this code can be found in Program.cs. To accept parameters you will need to modify this method so that it accepts a string array as a parameter.  Most people name this parameter "args."

``` csharp
[STAThread]
static void Main(String\[\] args)
{
    ...
}
```

If a parameter was passed, args will have a length greater than zero.  To retrieve the command line arguments all you need to do is retrieve the parameters out of args.

``` csharp
[STAThread]
static void Main(String\[\] args)
{

    if (args.Length > 0)
    {
        string arg1 = args\[0\];
        // do something appropriate with
        // arg1 here. }
    Application.EnableVisualStyles();
    Application.SetCompatibleTextRenderingDefault(false);
    Application.Run(new Form1());
}
```
