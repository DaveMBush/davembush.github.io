---
title: 'C# Properties Get and Set'
tags:
  - .net
  - 'c#'
  - get
  - properties
  - set
url: 2727.html
id: 2727
categories:
  - 'c#'
date: 2014-11-13 07:00:00
---

My son is learning to program.  Last week he asked me to explain C# properties get and set and, as it turns out, it looks like many others are asking for the same.  So, I’ve decided to spend the time on this post, explaining getters and setters in about as much detail as one can expect. So here it goes…

Member Variables
----------------

So, a class has "member variables" that are typically scoped as private, although they could be (but shouldn’t be) scoped as public.

class SomeClass
{
    private int _someMemberIntegerVariable;
}

Inside the class definition but not in the methods.  You’ll also sometimes see this referred to as a “field”.  I call them “member variables” because that is what I learned them as back when I was programming C++.

Local Variables
---------------

If the variable is in a method, it is called a "local variable"

class SomeClass
{
    public void SomeMethod()
    {
        int someLocalIntegerVariable;
    }
}

Class “State”
-------------

Now, the reason we have member variables is because they hold the "state" of the object.  For example, you might have a person class (typical example).  The Person class would have a firstName, lastName, birthDate as member variables so that when the class is created (and becomes an object) they can hold the state of the person. "Dave", "Bush", 6/20/61.

class Person
{
    private string _firstName;
    private string _lastName;
    private Date _birthDate;
}

Old Time Get and Set
--------------------

Now, in the old days (my C++ days) we'd just make those member variables public so that any other class could access them directly.  The problem with that is that any other class could access them directly leaving our class unable to control what exactly came into them.  And so, some gatekeeping was added.  In C++ and Java, that was done with setter methods and getter methods.  In Java that may have changed since the days I programmed in Java, but they started that way at least. That is, setFirstName(string name), setLastName(string name), setBirthDay(date birthday)

class Person
{
    private string _firstName;
    private string _lastName;
    private Date _birthDate;

    public setFirstName(firstName)
    {
        _firstName = firstName;
    }
    public setLastName(lastName)
    {
        _lastName = lastName;
    }
    public setFirstName(Date birthDate)
    {
        _birthDate = birthDate;
    }
}

  and to retrieve them.... getFirstName(), getLastName(), getBirthDate() the setters and getters are public (or protected, or private as needed) but the member variables are always private so that only the class they are declared in can access them. Inside the setter, we make sure the data is valid before we set the member variable, or possibly do some sort of computation before we store it, or even pass it on to some other location. But as far as anyone using the class is concerned, it is set setting a value and when it calls the getter, it retrieves the value, or something similar.

C# Properties - Get and Set
---------------------------

So along comes C# and that language says, "having a getter method and a setter method is pretty dumb, we should syntactically stich them together." And so they came up with properties The syntax for that is

private datatype _propertyName;
public datatype PropertyName
{
    get {return _propertyName;}
    set{_propertyName = value;}
}

Which is all declared inside a class.  The member variable doesn't have to be named the same as the property, but it often is .  It is customary to name member variables with a leading underscore.  Local variables start with a lowercase character. So, properties and member variables are distinct. Although, you may have thought they were essentially the same thing. At the end of the day, once they are compiled, properties are just methods. But syntactically, you access them as though they were variables.  In fact, if you looked at a property in Intermediate Language (IL), the language that all .NET code compiles to, you would see that it is just a method. To access a property from within your code, you would access it as

someObject.FirstName = "Dave";

or

someVariable = someObject.FirstName;

the only reason they exist at all is to keep the outside world (outside the class) from stomping on the member variables of the class directly. The compiler does for us what the old timers did (and the Java guys still do) using getMethod and setMethod So, as it turns out, we need all that gatekeeping, but the fact of the matter is, many times we don't. When I was teaching, I'd have guys say, "if I don't need the gatekeeper, why even bother with the properties?" Which is kind of a valid point But I always countered, "But what if you eventually do?

Enhanced Properties
-------------------

I think Microsoft heard that so they embellished the language so that we don't have to declare the member variable if all we are going to do is just pass the data on through to it. That syntax is

public dataType PropertyName {get;set;}

the compiler generates the member variable for you So if I wanted to actually do something with the member variable, you would need to declare the member variable. It depends on what you were going to do with it. If you just wanted to retrieve the data at some other point in your application, you'd just use the property.  But if you needed to manipulate the data as it was being set or retrieved, you'd have to use the original syntax. And that's properties up until today.