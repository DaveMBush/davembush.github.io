---
title: Value Type vs Reference Type
tags:
  - 'c#'
  - objects
  - programming
  - reference types
  - value types
url: 2743.html
id: 2743
categories:
  - 'c#'
date: 2014-11-27 07:00:00
---

It is amazing to me how few programmers understand the fundamentals of how variables work.  Not just in .NET or C# specifically, but in every language they work in.  It amazes me for two reasons.  First, I don’t think I could program if I didn’t understand what was physically happening as a result of the code I was writing.  Not knowing how the variables relate to the memory that they use would be, to me, a major limitation.  But it also amazes me because I don’t think anyone can program intelligently until they do know what is happening. So, I’ll start from the outside and move in to what’s happening in memory.

What is A Value Type
--------------------

The first question we need to qualify is, “What types in .NET are referred to as Value types?  Common value types are int, double, float, decimal, and bool.  What we ypically refer to as “primitives”.  But, there are other types that are also value types.  Enums, structs, and DateTime(because it is a struct) are also value types.

What is a Reference Type
------------------------

Reference types are any types in .NET that derive from a Class and require the “new” keyword in order to have an instance of a variable of that type. Why didn’t I just say, “any type that derives from a Class?”  Well, the fact of the matter is that every type in .NET derives from a Class.  The top most class is “Object”.  All of the value types derive from the subclass of Object named, “System.ValueType”.

What Happens In Memory
----------------------

But it is what happens in memory when we use these variable types that is of interest to us. When you declare a variable that is a Value type and then assign a value to it, the memory that variable occupies holds the value you assigned to it.  The variable is just a representation of the actual value. Contrast this to a reference type.  When you new up (instantiate) a variable that is a reference type, the first thing that is happening is that memory is being allocated to hold the variables in the class and then memory is being set aside to hold a pointer to the memory we just allocated. So, with a reference type, we are only pointing to the memory we are actually using.  With a value type the variable IS the value we are using. This has implications to how the memory is used when we do assignments.

Value Example
-------------

For example, look at the code below.

int a = 1;
int b = 2;

a = b;

When we assign b to a, we are copying the value occupied by b into the memory location occupied by a.

Reference Example
-----------------

But what happens when we do the same thing with a reference type?

class Person
{
    string Name;
    int age;
}

var joe = new Person();
joe.Name = "Joe";
joe.age = 23;

var alice = new Person();
alice.Name = "alice";
alice.age = 33;

joe = alice;
joe.age = 50;

What will be the value of alice.age? You should say 50 because once we assigned alice to joe, alice and joe point to the same Person object and the Person object that alice pointed to is no longer available.

How About Structs?
------------------

But what happens if we make the Person class a struct instead?

struct Person
{
    string Name;
    int age;
}

Person joe;
joe.Name = "Joe";
joe.age = 23;

Person alice;
alice.Name = "Alice";
alice.age = 33;

joe = alice;
joe.age = 50;

Now, what is the value of alice.age? In this case, you should say that alice is still 33 because when we assigned alice to joe, joe got a copy of everything that alice had. So, joe’s name is “alice” and before we asign 50 to joe.age, joe.age holds the value of 33.  But the assignment has no impact on the value of alice.age.

Stacks And Heaps
----------------

Now, no description of value types and reference types would be complete without some discussion of stacks and heaps. The stack is the location in memory that holds value types and reference pointers (remember I said the variable points to the memory being occupied by the value?) in your method. So when you declare a variable inside of a method that memory gets “Pushed” onto the stack. When you pass a variable to another method, that variable gets copied into a temporary variable and placed on the stack. So, doing something like

void Foo()
{
    int i = 23;
    Foo2(i);
}

void Foo2(int f)
{
   // do something with f
}

will copy 23 so that the variable f in Foo2 will not be the variable i in Foo. So if we change the value of f in Foo2 to 32, what will be the value of i when Foo2 returns? Because it is a copy, it will still be 23. The heap, on the other hand, is a location in memory that is outside of the scope of the methods we create.  So the only thing being passed around in our functions that use reference variables is pointers.  But, because they are pointers, any thing we do do a reference object inside of a method will be reflected in the variable located inside of the method that called it.

void Foo()
{
    var p = new Person();
    p.age = 24;
    Foo2(p);
}

void Foo2(Person person)
{
    person.age = 44;
}

So when Foo2 returns, p.age will be 44. However, if we change what person is pointing to…

void Foo2(Person person)
{
    person = new Person();
    person.age = 44;
}

p would remain unchanged and p.age would still be 24.

Values Inside of a Class
------------------------

The final question that you should be asking at this point is when I declare a value type as a member variable of my class, as I’ve done with the age variable in Person above, where is the age variable located, on the stack or on the heap? The answer to that would be it is located in the heap because it is a member of a class that is located in the heap.  And if we created another person object inside of person, the pointer would also be located in the heap and it would point to another location of the heap.