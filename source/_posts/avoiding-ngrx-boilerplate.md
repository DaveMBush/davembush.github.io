---
title: Avoiding NgRx Boilerplate
date: 2022-11-19 12:17:54
tags:
  - angular
  - ngrx
  - state-management
---

One of the recurring complaints I hear about NgRX is that it requires too much boiler plate code. But, it doesn't have to be this way. In fact, I've been working on a project that has a lot of NgRX code and I've been able to reduce the amount of boilerplate code to a minimum and gain features in the process.

<!-- more -->

If you look at your NgRX code you should find that your actions, reducers, and effects are all very similar. If you don't find this to be true in your own code, you should consider this a code smell. While how we each use NgRX may vary from project to project, within a project there should be enough consistency that you can reduce the boiler plate code you write and just supply the differences rather than repeating yourself over and over again.

## NgRX Entities

One reason you may find your code does not lend itself to common actions, reducers, and effects is because you have not yet discovered NgRX Entities. NgRX Entities are an "Optional" NgRX add-on that allow you to reduce quite a bit of redundant code, especially in the Reducers where you can just make a call to an Adapter function to do all the work for you. The end result is that your store is an array of IDs and an object of type `Record<string, T>` where `T` describes the shape of your record and `string` is the ID of the record. Using this pattern it is very easy to find a record by the ID so we can easily join to slices of our store together using a selector.

The remainder of this article assumes you are familiar with NgRX Entities and how to use them. If you are not, I recommend you read the [NgRX Entity Documentation](https://ngrx.io/guide/entity) and then come back to this article.

## Actions

The first, very obvious place we find boiler plate code in NgRX is with actions. The more recent release of NgRX already makes creating actions relatively easy by providing the `createActionGroup` function. This function allows you to create a group of actions with a single function call. However, it still requires you to provide the action type and the action payload. This is where we can start to reduce the boiler plate code.

Let's say, for a start, that all your actions look something like this:

```typescript
export const entityActions = createActionGroup({
  source: 'Entity Name Here',
  events:{
    Load: props<{ id: string }>(),
    LoadSuccess: props<{ entity: Entity }>(),
    LoadFailure: props<{ error: string }>(),
    LoadAll: props<{ ids: string[] }>(),
    LoadAllSuccess: props<{ entities: Entity[] }>(),
    LoadAllFailure: props<{ error: string }>(),
    Create: props<{ entity: Entity }>(),
    CreateSuccess: props<{ entity: Entity }>(),
    CreateFailure: props<{ error: string }>(),
    Update: props<{ entity: Entity }>(),
    UpdateSuccess: props<{ entity: Entity }>(),
    UpdateFailure: props<{ error: string }>(),
    Delete: props<{ id: string }>(),
    DeleteSuccess: props<{ id: string }>(),
    DeleteFailure: props<{ error: string }>(),
  }
});
```

Even if you create some generator to create this code for you, it is still quite a bit of code that needs to be compiled and shipped to the browser.

What if, instead, all we had to do was write this:

```typescript
export const entityActions = createActionGroup(
  'Entity Name Here'
);
```

All you need to do, for this to work, is create a new `createActionGroup` function that returns the NgRX version of the same method with everything filled out.

Well, "all you have to do" is a bit of an exaggeration as there are some typing issues you'll need to address.

Here is the code:

```typescript
import { createActionGroup as ngrxCreateActionGroup, props } from '@ngrx/store';

import { NgrxActionGroup } from './types/ngrx-action-group.type';

/**
 * StringLiteralCheck was copied from NgRX because it is hidden.  It forces
 * the source parameter to be a string literal.
 */
type StringLiteralCheck<
  Str extends string,
  Name extends string
> = string extends Str ? `${Name} must be a string literal type` : unknown;

type StringLiteralSource<Source extends string> = Source &
  StringLiteralCheck<Source, 'source'>;

/**
 * createActionGroup creates all the actions an entity will need reducing quite a bit
 * of boilerplate code.
 *
 * The `source` parameter is the same as the `source` parameter in NgRX's createActionGroup.
 */
export const createActionGroup = <Source extends string, T>(
  source: StringLiteralSource<Source>
): NgrxActionGroup<T> =>
  ngrxCreateActionGroup({
    // because ngrxCreateActionGroup expects a string literal
    // we have to cast as any to get around the type check
    // this is why we made sure our source parameter does the
    // string literal check.
    source: source as any,
    events: {
      Load: props<{ id: string }>(),
      LoadSuccess: props<{ entity: T }>(),
      LoadFailure: props<{ error: string }>(),
      LoadAll: props<{ ids: string[] }>(),
      LoadAllSuccess: props<{ entities: T[] }>(),
      LoadAllFailure: props<{ error: string }>(),
      Create: props<{ entity: T }>(),
      CreateSuccess: props<{ entity: T }>(),
      CreateFailure: props<{ error: string }>(),
      Update: props<{ entity: T }>(),
      UpdateSuccess: props<{ entity: T }>(),
      UpdateFailure: props<{ error: string }>(),
      Delete: props<{ id: string }>(),
      DeleteSuccess: props<{ id: string }>(),
      DeleteFailure: props<{ error: string }>(),
    },
  });
```

You'll notice that I've added a new type `NgrxActionGroup` as the return value. This ensures that the return value is strongly typed to the information provided.

``` typescript
export type NgrxActionGroup<T> = ActionGroup<
  // using any here is the only way I could get this to work
  // without worse hacks or rewriting createActionGroup
  any,
  {
    Load: ActionCreatorProps<{ id: string }>(),
    LoadSuccess: ActionCreatorProps<{ entity: T }>(),
    LoadFailure: ActionCreatorProps<{ error: string }>(),
    LoadAll: ActionCreatorProps<{ ids: string[] }>(),
    LoadAllSuccess: ActionCreatorProps<{ entities: T[] }>(),
    LoadAllFailure: ActionCreatorProps<{ error: string }>(),
    Create: ActionCreatorProps<{ entity: T }>(),
    CreateSuccess: ActionCreatorProps<{ entity: T }>(),
    CreateFailure: ActionCreatorProps<{ error: string }>(),
    Update: ActionCreatorProps<{ entity: T }>(),
    UpdateSuccess: ActionCreatorProps<{ entity: T }>(),
    UpdateFailure: ActionCreatorProps<{ error: string }>(),
    Delete: ActionCreatorProps<{ id: string }>(),
    DeleteSuccess: ActionCreatorProps<{ id: string }>(),
    DeleteFailure: ActionCreatorProps<{ error: string }>(),
  }
>;
```

Most of the work in getting that all working correctly was with the strong typing. You may need to tweak for your purposes, but this is the basic idea.

If you need to add actions that are unique to a particular entity, you can use the `createActionGroup` function from NgRX and then return the combined actions using the spread operator.

```typescript
export const entityActions = {
  ...createActionGroup('Entity Name Here'),
  ...ngrxCreateActionGroup({
    source: 'Entity Name Here',
    events: {
      UniqueAction: props<{ id: string }>(),
    },
  })
};
```

## Reducers

Reducers are very similar. Once again, NgRX already gives us a way of creating a Reducer function with one call.

```typescript
export const entityReducer = createReducer(
  initialState,
  on(entityActions.Load, (state, { id }) => {
    return {
      ...state,
      loading: true,
    };
  }),
  // etc...
);
```

But if the on() code is the same, why repeat it over and over?  Just create a new factory function that specifies what is unique and let it do the rest of the work.

```typescript
export const createReducer = <T, N extends string>(
  adapter: EntityAdapter<T>, // EntityAdapter is an NgRX Entities type
  entityName: N,
  actions: NgrxActionGroup<T>
): ActionReducer<EntityState<T>> => {
  return ngrxCreateReducer(
    adapter.getInitialState(),
    on(actions.load, (state, { id }) =>
      adapter.upsertOne(id, state)
    ),
    // etc...
  );
};
```

And now, you can create all your reducers using your new createReducer function.

```typescript
export const entityReducer = createReducer(
  entityAdapter,
  'Entity Name Here',
  entityActions
);
```

## Effects

Effects are a little more complicated. The basic idea is that you want to create an generic class with abstract methods it can use to pull in what is different from the class that implements it. To be honest, there is still quite a bit of code you have to write but there are two advantages. Since you are only providing the differences, there is much less code to write and because it is the same code being used everywhere, once you get it working, it works everywhere, reducing the chance of errors.

Because your code is probably going to look much different than the code I'm using, I'm going to provide samples of the code rather than the full blown implementation.

This also takes advantage of the new createEffect() function in NgRX 8.0.0.

Our generic class is going to look something like this:

```typescript
export abstract class SmartEffects<T> {
  /**
   * actions is the action group for this entity
   */
  protected abstract get actions(): NgrxActionGroup<T>;
  /**
   * adapter is the entity adapter for this entity
   */
  protected abstract get adapter(): EntityAdapter<T>;
   /**
   * updateOperator is the method that will run when the update
   *   action is dispatched.  You would create a similar operator
   *   method for any other calls you'd make to the server or other
   *   insertion points you would need for an effect
   * @param action
   */
  protected abstract updateOperator(
    // have to use any here because that is how NgRX defines it
    action: {row: T} & TypedAction<`[${any}] Update`>
  ): Observable<T>;
}
```

You might thing you could use an abstract field instead of  an abstract getter. But, javascript initializes the parent class prior to the child class and the field you need won't be available when the effects are created.

You'll need to make the calls to the createEffect() methods inside the constructor. Again, the order that code gets executed between parent classes and child classes forces this to be done in the constructor.

## Conclusion

Once you've created a consistent way of using NgRX throughout your code, you not only reduce the amount of boiler plate code you have to write, but you also reduce errors, make your code more maintainable, and create consistency throughout your code which will make it significantly more understandable.
