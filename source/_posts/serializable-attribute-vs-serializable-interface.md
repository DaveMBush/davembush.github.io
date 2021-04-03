---
title: Serializable attribute vs Serializable Interface
tags:
  - 'c#'
  - serialization
url: 133.html
id: 133
categories:
  - 'c#'
date: 2008-04-09 07:11:33
---

Judging from the comments I received yesterday, it looks like we need to review  serialization in .NET.

## The Easy Way

There are two ways of making an object serializable.  The first, and easiest way is by applying the Serializable attribute to the class.  Once you've done this all of the member variables in the class will get stored into the stream unless you've attributed one of the variables with NonSerialized.

This method is useful for objects you want to be able to store temporarily.  For example, you might use this to transfer objects using Web Services or if you wanted to store the object in a session variable when your sessions are being stored in a session server.  You would never use this to store the object to a file that you want to be able to retrieve some time in the future after restarting your application.  That is, you would never use this method to implement the [Registry solution](/2008/04/08/unauthorizedaccessexception-writing-to-hklm/) I mentioned yesterday.

## The Reliable Way

What you need instead is the Serializable Interface, or more correctly, you want to implement ISerializable on your class.  Using the ISerializable interface gives you a lot more control.  Used correctly, you can store an object to a file system, shut down your application, install an upgrade to the application with additional information (or less information) in the class, and still successfully load the object from the file system.  So, let's illustrate with a simple class:

``` csharp
class Demo {
    private int m_intVar;
    public Demo()
    {
    }
}
```

## The Basics

This is a pretty simple class.  It has one member variable that we will use to save some sort of state information and will need to get stored to the stream when we save the object instance.  To implement the interface, we need to change the class to look like this:

``` csharp
[Serializable]
class Demo : ISerializable {
    private int m_intVar;
    public Demo()
    {
    }

    public Demo(SerializationInfo info, StreamingContext context)
    {
        m_intVar = info.GetInt32("m_intVar");
    }

    public void GetObjectData(SerializationInfo info,
        StreamingContext context)
    {
        info.AddValue("m_intVar", m_intVar);
    }
}
```

The thing to watch out for here is that you need a constructor that takes SerializationInfo and StreamingContext as parameters.  Since the interface can't remember this, you'll have to remember it.

You'll notice that we've taken control of storing and retrieving the data by using the AddValue and GetInt32 calls inside of the new, required, constructor and the GetObjectData method.  GetObjectData is called when the data is being saved.  The constructor gets called when the data is being loaded.

## Protecting For The Future

If we leave the code as it is above, we are no better off than if we had simply implemented the Serializable attribute.  What we need to add to this code is some way of versioning the object.  This is easy to do by adding a few more lines of code: When we initially create the object, all we need to do is add one line of code to our GetObjectData method

``` csharp
public void GetObjectData(SerializationInfo info,
    StreamingContext context)
{
    info.AddValue("VERSION", 1);
    info.AddValue("m_intVar", m_intVar);
}
```

## The Future

Some time later, we decide to add another variable to our class:

``` csharp
private int m_intVar;
private string m_stringVar;
```

And save it as part of GetObjectData.  Because we've added another variable, we need to increment the version number.  So, GetObjectData now looks like:

``` csharp
public void GetObjectData(SerializationInfo info, StreamingContext context)
{
    info.AddValue("VERSION", 2);
    info.AddValue("m_intVar", m_intVar);
    info.AddValue("m_stringVar", m_stringVar);
}
```

and our constructor now looks like:

``` csharp
public Demo(SerializationInfo info, StreamingContext context)
{
    int VERSION = info.GetInt32("VERSION");
    m_intVar = info.GetInt32("m_intVar");
    if (VERSION > 1)
        m_stringVar = info.GetString("m_stringVar");
    else m_stringVar = "default string";
}
```

By incrementing the version number every time we make a change to the class and testing for the appropriate version number when we load the object, we can avoid having our application crash simply because the class has changed between the time we save the object and the time we loaded the object from the stream.
