---
title: What I Learned Writing SmartNgRX
date: 2024-10-14 7:12:03
tags:
  - ngrx
  - angular
  - smart-ngrx
---

Last week, I announced a project I had been working on for over 10 months. I call it [SmartNgRX](/smart-ngrx/) and it solves many common issues most of us have using SmartNgRX, including the boilerplate issue, over-fetching, and memory pressure caused by stale data.

Today I want to talk about a few things I learned while creating SmartNgRX that can be applied globally to any project.

<!-- more -->

## No Boilerplate Needed

While SmartNgRX solves the boilerplate issue, you can apply this to your code simply by making all your NgRX slices behave the same. Once you've done that, you can create a single factory function for each of your Actions, Reducers, and Effects and use that to generate the Actions, Reducers, and Effects you need.

You can see how I've done this by looking at the source code for SmartNgRX.

### Generic Actions

``` typescript
const actionGroupCache = new Map<string, unknown>();

export function actionFactory<T extends SmartNgRXRowBase>(
  feature: string,
  entity: string,
): ActionGroup<T> {
  const source = `${feature}${psi}${entity}`;
  const cached = actionGroupCache.get(source) as ActionGroup<T> | undefined;
  if (cached) {
    return cached;
  }

  const actionGroup = createActionGroup({
    source: source as any,
    events: {
      'Update Many': props<UpdateChanges<T>>(),
      Remove: props<IdsProp>(),
      'Load By Ids': props<IdsProp>(),
      'Load By Ids Preload': props<IdsProp>(),
      'Store Rows': props<RowsProp<T>>(),
      Update: props<{ old: RowProp<T>; new: RowProp<T> }>(),
      'Add To Store': props<RowProp<T>>(),
      Add: props<{
        row: T;
        parentId: string;
        parentFeature: string;
        parentEntityName: string;
      }>(),
      'Add Success': props<{
        newRow: T;
        oldRow: T;
        parentId: string;
        parentFeature: string;
        parentEntityName: string;
      }>(),
      Delete: props<{
        id: string;
        parentInfo: { feature: string; entity: string; ids: string[] }[];
      }>(),
    },
  });
  actionGroupCache.set(source, actionGroup);
  return actionGroup;
}
```

A casual glance will show you that all I've done is wrap the `createActionGroup` function from NgRX with a function that takes a feature and entity name and returns an ActionGroup. This is a simple example of reducing boilerplate in your code. We use the feature name and the entity name to generate the source name for the ActionGroup which makes it unique.

You'll notice that I've also cached the `ActionGroup` so that if you call it again, you'll get the same `ActionGroup` back. This simple optimization allows you to use the factory function to retrieve the action wherever you need it instead of passing it around. In my case, I needed this because the library doesn't expose the actions used by NgRX and I wanted to let the developers using SmartNgRX use the actions in their code if they desired.

### Reducers

Similarly, I've wrapped `createReducer` with a factory function that takes the feature and entity name and returns a reducer. Again, this is all you need to ensure the reducer is unique.

``` typescript
export function reducerFactory<T extends SmartNgRXRowBase>(
  feature: string,
  entity: string,
): ActionReducer<EntityState<T>> {
  const adapter = entityDefinitionCache<T>(feature, entity).entityAdapter;
  const initialState = adapter.getInitialState();
  const actions = actionFactory<T>(feature, entity);

  return createReducer(
    initialState,
    on(actions.add, (state, { row }) => adapter.upsertOne(row, state)),
    on(actions.addSuccess, (state, { newRow }) =>
      adapter.upsertOne(newRow, state),
    ),
    on(actions.updateMany, (state, { changes }) => {
      return adapter.updateMany(changes, state);
    }),
    on(actions.remove, (state, { ids }) => adapter.removeMany(ids, state)),
    on(actions.update, (state, { new: { row } }) =>
      adapter.upsertOne(row, state),
    ),

    on(actions.storeRows, (state, { rows }) => adapter.upsertMany(rows, state)),
  );
}
```

### Effects

Finally, I've wrapped `createEffect` with a factory function that takes the feature and entity name and returns an effect. This is the same as the other two examples.

``` typescript
export function effectsFactory<T extends SmartNgRXRowBase>(
  feature: string,
  entityName: string,
  effectsServiceToken: InjectionToken<EffectService<T>>,
): Record<EffectsFactoryKeys, FunctionalEffect> {
  const actions = actionFactory<T>(feature, entityName);
  const entityDefinition = entityDefinitionCache<T>(feature, entityName);
  const adapter = entityDefinition.entityAdapter;
  return {
    /**
     * Ends up calling the `EffectService` to delete the row specified
     * by the ID in the action.
     */
    delete: createEffect(
      deleteEffect(effectsServiceToken, actions),
      dispatchFalse,
    ),
    /**
     * Ends up calling the `EffectService` to determine what rows
     * need to be loaded yet and returns dummy rows for those rows.
     */
    loadByIdsPreload: createEffect(
      loadByIdsPreloadEffect(feature, entityName, actions),
      dispatchFalse,
    ),
    /**
     * Ends up calling the `EffectService` to load the rows specified
     * from the server.
     */
    loadByIds: createEffect(
      loadByIdsEffect(effectsServiceToken, actions, feature, entityName),
      dispatchFalse,
    ),
    /**
     * Ends up calling the `EffectService` to update the row specified
     * by the row in the action.
     */
    update: createEffect(
      updateEffect<T>(effectsServiceToken, actions, feature, entityName),
      dispatchFalse,
    ),
    /**
     * Ends up calling the `EffectService` to add the row specified
     * by the row in the action.
     */
    add: createEffect(addEffect(effectsServiceToken, actions), dispatchTrue),
    /**
     * Handles adding the new row to the store and removing the dummy row
     * that was added so we could edit it.
     */
    addSuccess: createEffect(
      addSuccessEffect<T>(effectsServiceToken, actions, adapter),
      dispatchFalse,
    ),
  };
}
```

By now, you get the general idea. A couple of things to note about the `effectsFactory`.

First we pass in an `InjectionToken` that is used to get the `EffectService` that is used by the effects. By doing this, all we need to do is make sure your effects services conform to the `EffectService` abstract class and we can use them in the effects.

This is how SmartNgRX can hide NgRX from the developer and just let them implement the specifics for each entity.

You'll also notice that to keep the code clean, I've implemented the details in other functions that I call. If you want to see the details, you can [look at the source code for the effects in SmartNgRX](https://github.com/DaveMBush/SmartNgRX/tree/cbec5143855b1916e0c67776b22a6134a18d5ee4/libs/smart-ngrx/src/effects).

Second, we use the cached `ActionGroup` by calling the `actionFactory` function to get the actions we need for the effects instance. Now we don't need to know about a specific instance and find some way to pass it into the effect.

Finally, we need access to the EntityAdapter for the entity we are working with. We create the entity adapter when we configure SmartNgRX. The two lines of code, above, that retrieve the adapter are retrieving it from a map where we set it during SmartNgRX setup

And that's how you avoid NgRX boilerplate in your code.

## Too Many Actions and Reducers

I've been saying this for several years and still managed to get this "wrong" while writing my code.

Here's the basic issue. Reducers and Effects have specific purposes. Where I went wrong is with my reducers. But I'll address both Reducers and Effects here.

But first, let's take a detour and talk about an ideal NgRX setup.

### Ideal NgRX Setup

The Redux pattern, generally, and NgRX, specifically, were created to store states predictably. "State only" is my motto. This means that logic should be kept out of our NgRX code. This keeps our NgRX code simple and reduces bugs. You wouldn't believe some of the code I've seen that violates this principle. Then again, maybe you could. You've probably written it.

So, if we want to keep logic out of NgRX, where should it go?

### Action Services

It goes in a service. When you call an action, it should already have the data in the form that NgRX ultimately needs. The reducer can then take this data and store it in the store without doing anything to it.

Notice how small my reducer code is.

### Effects Service

Similarly, when you call an effect, it should already have the data to do whatever it needs. Again, in an ideal world, all the effect does is call what I've come to term the effect service. The effect service can do whatever work needs to be done.

For example, say you are calling the server, but the shape of the data you get back isn't what your application will need. The place to do the transformation is in the effect service. Not in the effect.

By doing this, you'll only need actions that perform basic CRUD operations. You shouldn't see actions for every possible way you could change the state of your application. Most of that can, and should, be handled in an Action Service.

There has been a lot of noise in the community about using the "facade" pattern with NgRX but most implementations I've seen just put a class in front of the same actions we've always had. This is not a facade pattern. This is just a bulky, excessive layer in front of what we've always been doing.

What I'm doing is closer to a facade pattern because now all those action calls become calls to a service. The service does the transformations and dispatches whatever action is appropriate to update the store.

### Increased Performance

By using the facade pattern as I've laid out above, you also have the potential of increasing the performance of your application. Another motto I've developed is "keep as much out of the NgRX stream as possible." What that means is that if you can fire one action instead of more than one, you should.

Why is this? Because every action has to be handled by every effect and reducer in your code even if it doesn't need it. Think of every ofType() call as an IF that has to be evaluated. And, similarly, every on() call in the reducers is another IF that has to be evaluated. These add up.

### Command or Event Pattern

By following these tip, the question of using the command or event patterns with NgRX becomes moot. The argument for using the event pattern is that your actions now give you some idea of what triggered them by using the action name as the where. In the pattern I've described, 100% of your actions (should) get triggered by the same code every time if you've structured things correctly.  That code gets triggered by multiple other places just like any other function in your code does. Now, you can evaluate the call stack. You can still use the event pattern. But, there isn't a strong argument for it anymore.

The best part about using this pattern is that I'm in a great position to convert SmartNgRX to use Signals instead of Observables and most of my code won't change.

As far as NgRX code is concerned, the only NgRX place you should have any logic at all is in Selectors, but that's a topic for another day.
