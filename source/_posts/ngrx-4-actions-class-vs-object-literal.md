---
title: NgRX Actions - Class vs Object Literal
tags:
  - angular
  - NgRX
url: 4462.html
id: 4462
comments: false
categories:
  - Angular
date: 2018-07-03 06:30:58
---

When NgRX 4 came out and I discovered that the "right" way of creating Actions is to use TypeScript classes and not Object Literals, I was a bit surprised.  Why would you use a Class that requires you to use the "new" keyword?  Why would you put multiple classes in one file?  This is insane! <figure>![](/uploads/2018/07/2018-07-04.png "NgRX Actions - Class vs Object Literal")<figcaption>Photo credit: [dkobras](//www.flickr.com/photos/dkobras/8342620023/) / [ CC BY-SA](//creativecommons.org/licenses/by-sa/2.0/)</figcaption></figure>

<!-- more --> 

Options
-------

An Action is an object that contains a type variable and optionally, a payload. Depending on how you code your action, the payload may or may not have “payload” as the variable name. In NgRX version 2, payload was an optional variable. To improve type checking, payload was removed from the Action interface. 

The official documentation for NgRX version 4 encourages us to create a class for each action we want to dispatch. 

Suppose you have a Wait reducer that needs a Start action and an End action.  The code might look something like:

``` typescript
import {Action} from '@ngrx/store';
export const START = 'Wait.Start';
export class Start implements Action {
  readonly type = START;
}

export const END = 'Wait.End';
export class End implements Action {
  readonly type = END;
}
```

To use this code in our reducer or effects you would import like this:

``` typescript
import * as Wait from './wait.actions';
```

And then we would dispatch the action using the store we’ve injected into our code. We’ll cover that later. But for now, the dispatch basically looks like this: 

``` typescript
this.store.dispatch(new Wait.Start());
```

If you need to pass other information to an Action, your constructor can accept them: 

``` typescript
export const ACTION_WITH_MESSAGE = 'Wait.ActionWithMessage';
export class ActionWithMessage implements Action {
  readonly type = ACTION_WITH_MESSAGE;
  constructor(public message: string) {}
}
```

This allows us to access the action payload as `Wait.message` instead of `Wait.payload`. Contrast this to putting the same code in a class with static methods as I've explained in previous articles:

*   [How to Upgrade to NgRX 4](/how-to-upgrade-ngrx-to-4-x/)

``` typescript
export class Wait {
  const START = 'Wait.Start';
  const END = 'Wait.Start';
  const ACTION_WITH_MESSAGE = 'Wait.ActionWithMessage';    
  start() {
    return {type: Wait.START};
  }
  
  end() {
    return {type: Wait.END};
  }
  
  actionWithMessage(message: string) {
    return {
      type: Wait.ACTION_WITH_MESSAGE,
      payload: message};
    }
  }
}
```
  
In summary, we can have multiple classes and new them up.  Or we can have one class with multiple static methods that return object literals.

Advantage Object Literal
------------------------

The main advantage to using the object literal way is that you don't need to create an object.  You also stay with the "One class, one file" model that is so common in Angular. On the surface, this seems to be a clear winner.

Advantage Classes
-----------------

But, because of the way we import the class bundles, the way we end up using the code looks nearly the same.  The main difference is that we must instantiate the class.  But we also get the option of having a different variable type for each action, we aren't forced to use a variable named "payload" to hold all the associated data.  Further, if you need multiple payloads, you can do that.  You aren't limited by how many parameters/member variables each of your Action classes use.  When using Effects, we can type the return value of the Effect to the Action we want it to return. 

In fact, it is this type safety that is the main reason we should be creating Actions using the Class method instead of the object literal method.

Tweaking Classes For Bigger Advantage
-------------------------------------

There is a further tweak we can make to using classes that will give us an additional advantage when we use the code in our Reducers. Instead of marking the action types as strings, we can make them enums.

``` typescript
import {Action} from '@ngrx/store';

export enum Types {
  START = 'Wait.Start',
  END = 'Wait.End'
}

export class Start implements Action {
  readonly type = Types.START;
}

export class End implements Action {
  readonly type = Types.END;
}
```

You'll see below that this will give us extra type safety in our Reducers.

Type Safety in Reducers
-----------------------

``` typescript
import {ActionReducer} from '@ngrx/store';
import * as Wait from './wait.actions';

// This could go in wait.actions.ts
export type Action = Wait.End | Wait.Start;
export function WaitReducer(
  state = 0, action: Action): number {
    switch(action.type) {
      case Wait..Types.START:
        return state + 1;
      case Wait.Types.END:
        return state - 1;
      default:
        return state;
    }
};
```

We've ensured that the only Actions we will get are from Wait.

Now, what you can't see here is that if the payload we were send were of different types, each case statement would automatically typecast the action to the proper type.  This only works with Enums.  If you try this with strings, it won't work the same way.

Type Safety in Effects
----------------------

``` typescript
import * as Wait from './wait.actions';

@Injectable()
export class WaitEffects {
  @Effect()
  start$: Observable<Wait.Add> =
    this.actions$
      .ofType(Wait.START)
      .switchMap((action: Wait.Add) =>
        Observable.timer(1).take(1)
      )
      .map((): Wait.Add =>
        ({type: Wait.ADD, payload: 1}));
        
  @Effect()
  end$: Observable<Wait.Add> =
    this.actions$
      .ofType(Wait.END)
      .switchMap((action: Wait.End) =>
        Observable.timer(1).take(1)
      )
    .map((): Wait.Add =>
      ({type: Wait.ADD, payload: -1}));
    
  constructor(private actions$: Actions){
  }
}
```

We've ensured that the only Action that get returned from either Effect is the Add action.

Tweak Object Literal Implementation
-----------------------------------

Now, you might think, "I can get all that type safety by creating a separate interface for each action type.  And this would be true.  But this would be even more files and the only thing to be gained is that your Action object gets created faster because you are using Object Literals.  I'm not sure the advantage is worth the pain.

Conclusion
----------

If you are converting an NgRX 2 site to NgRX 4, you'll probably want to follow the advice I originally gave for upgrading.  But, once you've upgraded, you'll want to move to using Classes so that you can take advantage of the stronger typings this will afford you.
