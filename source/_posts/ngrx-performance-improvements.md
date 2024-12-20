---
title: NgRX Performance Improvement
date: 2024-12-15 11:29:45
tags:
  - ngrx
  - ngrx-store
  - ngrx-entity
  - ngrx-effects
  - angular
---

Over the course of working on [SmartNgRX](https://github.com/DaveMBush/SmartNgRX), I've discovered a secret to making NgRX faster. A way that is so obvious now that I know about it, I'm surprised this isn't the recommended way to use NgRX.

As most discoveries occur, I'm building on the backs of giants. In this case, I'm specifically building on the back of the increasingly popular NgRX facade pattern (done well) and a statement with regard to Angular Signals that says "Don't use Signal Effects."

There are, of course, other minor improvements but these are the top two catalysts for what I want to share with you today.

<!-- more -->

## Facade Pattern

Several guys at the place I work now, have been using the NgRX facade pattern for a while. Even when I interview, people mention it. What most have never been able to convince me of is why I'd want to use it. Where I've gotten stuck is that most people use it as nothing more than a pass-through to plain old vanilla NgRX.

That is, they'll create a facade class that basically looks like this:

```typescript
export class [Name]Facade {
  constructor(private store: Store) {}

  [actionName]() {
    this.store.dispatch(new [Name]Actions.SomeAction());
  }
}
```

And create methods for each action.

If that's you, I'm sorry. You're doing it wrong.

But going back to my article about what I learned building (SmartNgRX)[https://davembush.github.io/what-i-learned-writing-smartngrx/], what you want to do in the facade pattern is to do everything that isn't related to NgRX in the facade and then dispatch the action from the facade to update the resulting state.

By doing this, you're able to keep your NgRX code clean and simple.

## Dispatch Time

In SmartNgRX, one of the things I do is buffer the dispatch actions that retrieve the IDs so I can dispatch them all at once. Originally, I put the buffering code in an effect. But one day I thought, "What if I buffer the IDs and THEN dispatch the actions?"  It turns out that is faster.

That's when I realized (and found out I was wrong - see below) that when you dispatch an action, NgRX has to run it through every typeOf() in your effects regardless of if the effect needs to process it or not.  It's a lot like the chick going around the farm yard asking "Are you my mother?" instead of mama chicken coming to the chick and saying "I'm your mother, come here and I'll take care of you."

Reducers are a bit different. If you are using the newest on() syntax for reducers, this translates to a map statement so you end up with the benefits of O(1) time. If you are using older syntax, you have an increasingly complex switch statement that is O(n) time until switch statements are optimized.

The take away from this, I thought, is that you want as few effects as possible in your code.

I almost titled this article "Ban NgRX Effects" but that wouldn't give you the full picture.

## Signals

And then I stumbled across a video about Signals where Alex Rickabaugh from the Angular Core team said "(Don't use Effects)[https://www.youtube.com/watch?v=aKxcIQMWSNU&ab_channel=TechStackNation]."

And then it struck me. If I'm using a facade, I don't need to use effects.

Think about why we typically use effects. Isn't it so that we can ultimately call a service that will grab data from the server? And then, what do we do with that data? We fire an action to update the state.

## Ban Effects

Instead, why not just call the service directly from our facade?

```typescript
export class [Name]Facade {
  constructor(private service: [Name]Service) {}

  [methodName]() {
    // For optimistic updates, dispatch an action to update the state here.
    this.service.[methodName]().pipe(
      // process data from the backend and then
      // dispatch an action to update the state
    ).subscribe();
  }
}
```

Even better would be to use promises:

```typescript
export class [Name]Facade {
  constructor(private service: [Name]Service) {}

  [methodName]() {
    // For optimistic updates, dispatch an action to update the state here.
    const resultawait lastValueFrom(this.service.[methodName]());
    // process data from the backend and then
    // dispatch an action to update the state
  }
}
```

Now, you don't need effects.

The additional beauty of this is that you should be able to use NgRX Signals instead of NgRX from your facade with very little effort.  The methods you put in withMethods() will be correspond to your Actions. computed() signals will be your selectors. Done. With some nice dependency injection added in, you can switch between the two with ease.

There are, of course, other improvements you can make to your code. For example, your code will generally run faster if you are using NgRX Entities for your state management and retrieve the data from the server as you need it. You could roll your own, but I've already done that work for you.  Check out the [SmartNgRX](https://github.com/DaveMBush/SmartNgRX) library.

## Timings

And then I went to actually prove everything I've said above.

I created a simple Angular application that dispatches an action to an effect and then had that effect call a service that would return an action asynchronously that would then be dispatched to a reducer.

I also added 50 other effects that just had ofType() to emulate the need to check for every action.

No matter how I ran this code, I could not see any timing difference between using one effect or 50 effects. So, my statement above that started me on this journey was wrong. We, evidently, do not suffer a performance hit based on the number of effects we have.

However, I did find some interesting performance marks that still indicate that you might want to call your effects services directly.

Along with the code I mentioned above, I modified the code to call the service directly and then dispatch the resulting action.

Here are the results from running the code on my computer:

|          | TL w/ Effects  | TL Direct | SL w/ Effects | SL Direct |
|----------|----------------|-----------|---------------|-----------|
| Average  | 176.2232ms     | 0.0158ms  | 0.3688ms      | 0.1875ms  |
| Max Time | 202ms          | 0.7000ms  | 1.1000ms      | 0.4000ms  |

> TL = Tight Loop (for/next loop)
> SL = Spaced Loop ( using rxjs of().pipe(repeat({count: 1000, delay: 100 })))

In both the tight loop and the spaced loop, the direct call outperforms dispatching to the effect. But, the difference is not significant for most applications and should be considered a micro-optimization.

On the other hand, if you are doing a lot of dispatching to effects at any one time, these difference could add up and you might want to consider using the direct call.

## Other Considerations

While banning effects for performance reasons may not be useful for most existing applications, there are other reasons you might want to consider banning them.

As I've moved from organization to organization, I've seen some pretty ugly NgRX code. Much of it is due to the complexity of effects. If the people using NgRX were not forced to use RxJS because of the effects, the code would be easier to read and we'd avoid a lot of the effect chaining that I've seen. While effects themselves aren't particularly slow, chaining the effects and using RxJS operators can be.

Since I've already removed the effects from SmartNgRX, and there is a slight performance benefit from doing so, I'm not going to revert that change. Especially since it should allow me to use 99% of the same code to implement the Signals version.
