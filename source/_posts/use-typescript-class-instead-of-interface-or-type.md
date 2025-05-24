---
title: Use TypeScript Class instead of Interface or Type
date: 2025-04-02 13:04:48
tags:
  - TypeScript
  - Performance
---

Since TypeScript introduced Interfaces and Types, we've been getting lazy. It is so much easier to create an object that obeys an interface than it is to create a Class that obeys the same interface. But what have we lost in the process?

<!-- more -->

## Checking Type at Runtime

In a typical application, if we want a strongly typed object, we create an interface or a type, define our object, and then assign an object to a variable with that type.

```typescript
interface User {
  id: number;
  name: string;
}

const user: User = {
  id: 1,
  name: 'John Doe',
};
```

All goes well until we need to check to see if the object is of type User. To do that, we have to test to see if the object has the properties we expect and that those properties have the types we expect.

If we had created a class instead, we could have used the `instanceof` operator to check if the object is of type User.

```typescript
class User {
  constructor(public id: number, public name: string) {}
}

const user = new User(1, 'John Doe');
const isUser = user instanceof User; // true
```

This is a much cleaner solution. We can also add methods to the class that operate on the properties of the class, which is not possible with an interface or type.

```typescript
class User {
  constructor(public id: number, public name: string) {}

  // Silly example
  getName(): string {
    return this.name;
  }
}
```

## But What About Flexibility?

But we've lost the flexibility of using an anonymous object to create the User. We can fix this by allowing the constructor to accept an object that has the same properties as the User class.

```typescript
class User {
  id: number;
  name: string;

  constructor(user: User) {
    this.id = user.id;
    this.name = user.name;
  }
}

const user = new User({ id: '1', name: 'John' });
```

In the example above, we've assigned the properties directly. But you could also use `Object.assign()` to copy the properties from the parameter into the class.

You might think that `Object.assign()` is the same as using the spread operator. But it is not.

If both objects in the spread or `Object.assign()` are objects without a class prototype, then yes, the change will be the same.

But if the target object is a class, then only the properties in the class will be moved over from the source object.

So, in code:

```typescript
class User {
  id: number;
  name: string;

  constructor(user: User) {
    Object.assign(this, user);
  }
}

const user = new User({ id: 1, name: 'John Doe', foo: 'bar' });
```

What ends up in the new object is only `id` and `name`. The `foo` property is ignored. This is not the case with the spread operator.

Well actually, you can't even use the spread operator in the constructor because it is not a valid syntax so we'll have to do this outside the constructor.

```typescript
class User {
  id: number;
  name: string;

  constructor(user: User) {
    Object.assign(this, user);
  }
}

const user = new User({ id: 1, name: 'John Doe'});

// then later on...

const user2 = { ...user, foo: 'bar' };
```

What we've done here is create a new object, `user2`, that has the same properties as `user` but with a new property, `foo`. This is not possible with the class constructor.

Not only that, but we've created an object literal based on the `User` class, but it is no longer a `User` object. It is just an object with the same properties as `User`. This means that if we try to use `instanceof` to check if `user2` is a `User`, it will return false.

```typescript

## Dev Tools Impact

An additional reason for using a class instead of an interface or a type is that the type information not only sticks around so we can use instanceof checks. But it sticks around so we can see the class name in our debug tools. This becomes important when you are looking at a stack trace that happened during runtime that you do not have map files available for or when you are looking at flame charts or you are tracking down memory leaks.

When you use an interface or a type, the type information is erased at runtime. This means that if you have a stack trace that shows an object of type User, you will not be able to see the class name in the stack trace. This can make it difficult to track down bugs and performance issues.

## Performance

And if you care about performance, there is one final reason you should want to use Classes instead of Interfaces or Types.

When you use an interface or type, it does not ensure that they fields stay in the same position as you copy it around.

What do I mean by this? And why do we care?

Let's use the same object example we've been working with but using interfaces again.

```typescript
interface User {
  id: number;
  name: string;
}

const user: User = {
  id: 1,
  name: 'John Doe',
};

const user2: User = { ...user, id: 2 };
```

We do this all the time, right? But what we've done is created one object, user, that looks like:

```typescript
{
  id: 1,
  name: 'John Doe',
}
```

And another object, user2, that looks like:

```typescript
{
  name: 'John Doe',
  id: 2,
}
```

That spread operator changed the order of our fields. Now, for applications that are small where we don't care all that much about performance, this doesn't matter. But, under the hood, the V8 engine is going to create a separate hidden class for each of these objects even though they are essentially the same type. The more fields you have in an object, the more hidden classes you are likely to create. This eats up memory as well as causing the V8 engine to have to do more work to optimize the code.

Now, above, I mentioned that we can use an anonymous object to initialize a class. To be clear, this is a concession to the fact that creating a new object with a lot of members is a pain and this makes it easier. You still have the same underlying issue that every variant of the object will create a new hidden class in V8. But, in most cases, you'll probably use the same shape, or at least a finite number of shapes, each time you create a new object and you are far less likely to use the spread operator on that anonymous object. You will need to decide which is more important to you: the performance of your application or the ease of creating new objects.

One of the next optimizations I'll be making in my code is to prefer Classes over Interfaces and Types. It is a trivial change to make and has huge benefits. I would encourage you to do the same.

## What about Interfaces and Types

Now, I'm not saying you should never use interfaces or types.  I'm saying you should not use them to define the shape of your objects. If you have other uses for them, such as using a type to define union types, then by all means, use them. But for defining the shape of your objects, you should be using classes.
