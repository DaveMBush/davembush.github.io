---
title: Implementing NgRX 4
tags:
  - angular 2
  - NgRX
url: 4474.html
id: 4474
comments: false
categories:
  - Angular 2
date: 2017-10-24 06:30:46
---

There seems to be a lot of confusion about Implement NgRX 4 in an Angular application.  Some of it I've contributed to because NgRX 2 isn't quite the same as NgRX 4 and as I've transitioned, I've learned better ways.  In other words, I was wrong and I'm correcting my mistake!  Below is the correct, NgRX approved way, of implementing NgRX 4. If you are looking for information about how to convert to NgRX 4 from NgRX 2, you can visit my previous article, [How to Upgrade to NgRX 4](/how-to-upgrade-ngrx-to-4-x/). Before we get started, make sure you have TypeScript 2.4.x or above installed in your local project.  The CLI may complain, depending on what version of it you are using.  But, NgRX 4 requires us to use TypeScript 2.4.x.  You should also have RxJS 5.4.x or above installed. You will also need to install @ngrx.  You can do this using the following NPM command: `npm install --save @ngrx/store @ngrx/effects` And finally, I've found that I need to use the --aot switch both when I'm using production and development builds, so you'll want to add that to your scripts in your `package.json` file. <figure>![](/uploads/2017/10/2017-10-24.jpg "Implementing NgRX 4")<figcaption>Photo credit: [cfaobam](//www.flickr.com/photos/cfaobam/12119410373/) via [VisualHunt.com](//visualhunt.com/re/300eff) / [ CC BY](//creativecommons.org/licenses/by/2.0/)</figcaption></figure>

<!-- more --> 

### Actions

An Action is an object that contains a type variable and optionally, a payload.  Depending on how you code your action, the payload may or may not have “payload” as the variable name.  In NgRX version 2, payload was an optional variable.  To improve type checking, payload was removed from the Action interface. The official documentation for NgRX version 4 encourages us to create a class for each action we want to dispatch. Assuming we have a Wait component we want to display when a counter is incremented and that should be removed when the counter returns to zero, you might want a Start wait action and an End wait action.  So, you would create a wait.action.ts file that has two classes in it.  A Start action and an End action. import {Action} from '@ngrx/store'; export const START = 'Wait.Start'; export class Start implements Action { readonly type = START; } export const END = 'Wait.End'; export class End implements Action { readonly type = END; } While this violates the "one class per file" rule, it actually provides us the ability to group our Actions together in one file, as you will see soon. To use these actions in our code, we would import them as a bundle: 

``` typescript
import * as Wait from './wait.actions';
```

This particular way of importing a packages groups all of the exported items under the variable named Wait.  Which allows us to dispatch the action using the store we’ve injected into our code using:

``` typescript
this.store.dispatch(new Wait.Start());
```

Start is an exported class from `wait.actions` that we've grouped under `Wait`. What makes this method of creating actions useful is that when we need to pass additional data along with our action, we can do that simply by adding parameters to our constructor.

``` typescript
export const ADD = 'Wait.Add';
export class Add implements Action {
  readonly type = ADD;
  constructor(public payload: number){}
}
```

We don't even have to call the payload, "payload."  How about calling it "value." 

``` typescript
export const ADD = 'Wait.Add';
export class Add implements Action {
  readonly type = ADD;
  constructor(public value: number){}
}
```

And if you need more than one payload item, you can pass in multiple parameters.

### Reducers

Reducers are functions that allow us to change the state of our entity within our application Store.  Reducers respond to the actions that have been dispatched and return a new object based on the changes requested. To continue on with our Wait example, `wait.reducer.ts` might look like this:

``` typescript
import {ActionReducer} from '@ngrx/store';
import * as Wait from './wait.actions';
// This could go in wait.actions.ts
export type Action =
    Wait.Add | Wait.End | Wait.Start;
export function waitReducer(
  state = 0, action: Action): number {
  switch(action.type) {
    case Wait.START:
      return state + 1;
    case Wait.END:
      return state - 1;
    case Wait.ADD:
      return state + action.payload;
    default:
      return state;
    }
};
export const WaitReducer:
  ActionReducer<number> = waitReducer;
```

The reason we write the `waitReducer` function and then assign it to the `WaitReducer` constant rather than putting the code all in one line is because the compiler will complain about our code otherwise. Notice that we have a ‘default’ in our switch statement.  Don’t forget to add this.  It isn’t an accident.  You see, when we register our Reducers and Effects with the store and then subsequently dispatch actions to them, all the reducers and effects get called.  In the case of reducers, if you don’t return something for each one of them, you’ll end up with a store that doesn’t know what kind of state it is in.  So, always return the current state as your default. The one thing that may not be obvious here is that the Reducer is a function.  Not a class.  This means you can’t inject other classes into it.  This is one of the reasons that the file is named `wait.reducer.ts` and not `wait.reducer**s**.ts`. But, Effects are different.

### Effects

Effects are classes.  Each Effect within the Effects class is a member variable.  So, we name our Effects file for Wait, `wait.effects.ts`. Now, remember I said that Effects were for things that caused side effects?  You may wonder, why kind of side effect does our Wait stuff need? Under normal circumstances, not any.  However, you can get into a situation in development mode where one of the safe guards Angular provides for you detects that you’ve changed the state of wait multiple times in a change detection loop.  Angular expects changes to be static. One way we can deal with this is to delay incrementing and decrementing to another change detection loop using setTimeout() indirectly. To do this, we are going to rip out the START and END case statements from our Reducer and add them into our Effects class. And then in `wait.effects.ts`:

``` typescript
import { Observable } from 'rxjs/Rx';
import { Injectable } from '@angular/core';
import { Actions, Effect } from``'@ngrx/effects';
import * as Wait from './wait.actions';
@Injectable()
export class WaitEffects {
  @Effect()
    start$: Observable<Wait.Add> =
      this.actions$
      .ofType(Wait.START)
      .switchMap(() =>
        Observable.timer(1).take(1)
      ).map((): Wait.Add =>
        (new Wait.Add(1)));
        
  @Effect()
    end$: Observable<Wait.Add> =
      this.actions$
      .ofType(Wait.END)
        .switchMap((action: Wait.End) =>
          Observable.timer(1).take(1)
        ).map((): Wait.Add =>
          (new Wait.Add(-1)));

    constructor(private actions$: Actions){
    }
}
```

You can name the member variables whatever you want.  They never get used.  `ofType()` is a filter that makes the observable stuff only fire for that particular type. The magic happens in the `Observable.timer(1).take(1)`.  Here we wait for one millisecond, take the first item out of the stream and immediately close the observable.  Once that completes, the `map()` returns a new action. Every Effect must return an action or you must tell the Effect that it won’t return an action.  Notice that we don’t dispatch the action.  That is done by NgRX for us.  We just return the action. To tell an Effect that no action will be returned, pass `{dispatch: false}` into `@Effect()`. `@Effect({dispatch: false}) …`

### Returning Multiple Actions from Effects

At the other end of possibilities, you may need to return multiple Actions from an Effect.  One way to do this is by using mergeMap().

``` typescript
.mergeMap((/* previous value here */) =>
{
  return [
    new ActionGroup.Action1(),
    new ActionGroup.Action2()
  ];
})
```

### Registration

Now, none of this is going to work if we don’t register our Reducers and Effects with the NgRX Store. If you look at a lot of the literature on how to write NgRX code, you’ll often see that they recommend that you put the code in AppModule.  That will work, but how much more effective to put the code for your Store in its own module.  So, I create a Module class called AppStores and put it in a file named `app.stores.ts`.  I know, technically it is a module and it should be `app-stores.module.ts`.  Using `app.stores.ts` isolates it from a normal module.  All I want to put in here is Store stuff. So, in my `app.stores.ts` file, I put code that looks like this: 

``` typescript
import { WaitEffects } from './wait.effects';
import { AppState } from './app.state';
import { WaitReducer } from './wait.reducer';
import { NgModule } from '@angular/core';
import { StoreModule, ActionReducerMap } from '@ngrx/store';
import { EffectsModule } from '@ngrx/effects';
const reducers: ActionReducerMap<AppState> = {
  wait: WaitReducer}

@NgModule({
  imports: [
    StoreModule.forRoot(reducers),
    EffectsModule.forRoot([WaitEffects])
  ]
})
export class AppStore { }
```

And the AppState interface that I used for my reducer map looks like this: 

``` typescript
export interface AppState {
  wait: number
}
```

A couple of things you should notice about this code.  First, the reducers object we create at the top of the file describes what our store looks like.  So far, we have an entity named “wait” that is controlled by our WaitReducer. Then to register the reducers with our application, we pass them on to `StoreModule.forRoot()` in the imports section of our module definition. Similarly, we must register our Effects with the application.  We do this by passing an array of Effects to `EffectsModule.forRoot()` in the imports section of our module definition. Of course, none of this code will even get included in your project unless you import this module into your AppModule class. 

``` typescript
@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    **AppStore**
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

Retrieving Data
---------------

Retrieving an entity from a store is quite simple.  Leaving out the imports that a good editor should help you with, here is the relevant code: 

``` typescript
export class AppComponent {
  wait: Observable<number>;
  constructor(store: Store<AppState>) {
    this.wait = store.select((s: AppState)
      => s.wait);
  }
}
```
And now you can subscribe to wait and whenever the value changes, you’ll get a notification and can do something with it.

Lazy-Loading NgRX
-----------------

Prior to NgRX version 4, all our state stuff had to live in the root of the application.  This was problematic because it also meant we were unable to easily locate our Actions, Reducers and Effects with the Routes they belonged to.  So, we had feature level components but everything else was more function based.  That is, all our State stuff lived together in one directory separate from the feature they supported.  Ugly! But now, we can create Reducers and Effects that live with the feature we support.

### Always Import forRoot()

The first thing you’ll need to be aware of is that even if you aren’t storing any state at the application level, you will need to call `StoreModule.forRoot()` and `EffectsModule.forRoot()` with an empty object and array. 

``` typescript
imports: [
  StoreModule.forRoot({}),
  EffectsModule.forRoot([])
]
```

### AppState vs FeatureState

You’ll remember that you’ll typically use an interface called AppState to define the structure of the Store for your application.  You are going to want to create a separate interface for each set of feature reducers you are loading.

### Feature Name as AppState Property

When you add a feature reducer, you’ll need to supply a name as the first parameter. 

``` typescript
imports: [
  StoreModule.forFeature('featureName',
    featureReducers),
  ...
]
```
Where `featureReducers` is the map of reducers for the feature.  Like what we did for the root level reducers. This “featureName” becomes the name of the store entity you’ll need to select to get at the store entities in your feature reducers. Imagine you have a feature named “featureName” as we’ve coded above and your `featureReducer` object has a feature property named “sub”.  Not super original, but it will do for an example. To select “sub” from your store, you would use code that would look something like this:

``` typescript
store.select(s =>
  s.featureName.sub);
```

This means that if you want to strongly type your selections using AppState, you will need to define a field in your AppState interface as “featureName” that is typed as the State interface for that feature.  Let’s call that FeatureState. 

``` typescript
export interface FeatureState {
  sub: SubModel;
}
```

And then our AppState would look like this: 

``` typescript
export interface AppState {
  featureName: FeatureState
}
```

And now we can write our select code like this: 

``` typescript
store.select((x: AppState) => x.featureName.sub);
```

If you have Effects that go with your reducers, you'll also need to import them with 

``` typescript
imports: [
  EffectsModule.forFeature([
    …list of effects here
  ])
]
```

### Features Without AppState

It is possible to access the feature state without putting it in your AppState.  This alternate method may be the only way you can access the code, but you might want to implement this method generally so that your features don't necessarily have to know about the application. 

To implement this access method, you use the `createFeatureSelector()`and `createSelector()` methods which you can import from `@ngrx/store`. The above code using these methods would look something like this: 

``` typescript
const featureSelector = createFeatureSelector<FeatureState>('featureName'); const subSelector = createSelector(featureSelector, (x: FeatureState) => x.sub));
store.select(subSelector);
```

Where store is injected using 

``` typescript
constructor(store: Store<FeatureState>){}
```

Note: I'm not suggesting that you write your code like this.  If you are repeating the `createFeatureSelector()` `createSelector()` code in multiple places, you should look for a way of not repeating yourself.  I've put it all in one place here, so you can see how they methods tie together in the bigger picture.
