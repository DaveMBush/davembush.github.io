---
title: Why Insist On Return Types In TypeScript?
date: 2023-03-25 14:47:38
tags:
  - typescript
  - strong typing
  - linting
---

There are different opinions about whether or not to use return types in TypeScript. I respect that, but I would like to share why I think they are helpful.

First, let’s acknowledge the reasons why some people prefer not to use return types.

## Reasons Against

### Verbosity and Redundancy

They can add verbosity and redundancy to the code, especially if the return type can be easily inferred by the compiler or the reader.

I understand that adding return types to methods and functions can make the code longer and sometimes repetitive. But I also believe that they can add some benefits that outweigh the costs. We’ll explore those benefits later.

### Limiting Flexibility and Reusability

They can limit the flexibility and reusability of functions, especially if the return type is too specific or restrictive.

I agree that we should avoid making our functions too specific or restrictive. But I don’t think that using return types necessarily leads to that. In fact, we can use generics and other TypeScript features to make our functions more flexible and reusable.

### Introducing Errors and Inconsistencies

They can introduce errors or inconsistencies if the return type does not match the actual implementation or behavior of the function.

I think this is a rare case, and if it happens, it means that we have a bug in our logic, not because of TypeScript but in spite of it. TypeScript is designed to catch these kinds of errors at compile time, so we can fix them before they cause any trouble.

### Sometimes difficult or impossible to specify

They can be difficult or impossible to specify for some functions that throw errors or have multiple return paths.

I admit that this can be challenging, but not impossible. We can use union types, never types, and other TypeScript tools to specify the return type for any function. If we can’t, then maybe we need to rethink our code design.

Is the function too big? Doing too much? Maybe we can split it into smaller functions that do one thing and do it well.

## Reasons For

If you haven’t noticed, I’m a fan of using return types in TypeScript. I think they make the code more readable, reliable, and maintainable. Here are some of the reasons why:

### Readability and Documentation

They make the code more readable and self-documenting, especially for complex or unfamiliar functions. They also help IDEs and editors to provide better code completion and hints.

By using return types, we can communicate to ourselves and other developers what a function is supposed to do and what it returns. This can save us time and effort when we need to understand or modify the code. It can also help us avoid mistakes and bugs by using the wrong type of data.

### Reliability and Robustness

They make the code more reliable and robust, especially for edge cases and unexpected scenarios. They also help TypeScript to [perform better type inference and type checking](https://www.typescriptlang.org/docs/handbook/type-inference.html) and [can improve the compile time performance](https://github.com/microsoft/TypeScript/wiki/Performance#using-type-annotations).

By using return types, we can ensure that our functions behave consistently and correctly, even when they encounter errors or unexpected inputs. We can also leverage TypeScript’s powerful type system to catch errors at compile time, rather than at runtime. This can prevent crashes and improve performance.

I recently read of a team who had a system that was largely untyped. Whenever a bug came in, they fixed the bug and then added types to the original code to see if the compiler would catch the bug. It turns out that simply adding types would have caught 15% of the bugs that came in.

### Maintainability and Scalability

They make the code more maintainable and scalable, especially for large or complex projects. They also help us to refactor and test our code more easily and confidently.

By using return types, we can make our code more modular and reusable, avoiding duplication and complexity. We can also refactor and test our code more easily and confidently, knowing that TypeScript will alert us if we break something.

### Sorting our Code Into Libraries

One problem I've run into in the past is with a code base that has a lot of typing issues. Along with that the code lived in 400+ libraries that is not very well sorted. I guess you have a similar problem only your code may not be quite so big.

In order to move the code to a system that allows us to cache our compile results the libs to be buildable. But you can only have buildable libs if none of the libs cause a circular lib reference. To do this, we needed a way to sort the code into appropriate libraries.

It might have been possible using inferred types, but it is considerably easier if we implicitly type our return values through out the code base.

Then, using AST, we can easily go through the code and determine what code needs to move and where we should move it to. It still isn't an easy job, but using return types makes it a lot easier.

## Conclusion

As you can see, I think that using return types in TypeScript is a good practice that can improve the quality of our code. Of course, there may be exceptions and trade-offs, depending on the context and preferences. But in general, I think that the benefits outweigh the costs.
