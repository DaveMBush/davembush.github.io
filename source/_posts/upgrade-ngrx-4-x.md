---
title: How to Upgrade NgRX to 4.x
tags:
  - angular
  - NgRX
  - RxJS
url: 4417.html
id: 4417
categories:
  - Angular 2
date: 2017-08-15 06:30:42
---

This week, I discovered that NgRX 4 had released. Yeah, we skipped over version 3 here too to get in sync with the Angular version numbering. There is a pretty complete upgrade guide that you can find [here](//github.com/ngrx/platform/blob/master/MIGRATION.md).  But, the upgrade guide assumes that you are creating Actions by instantiating a new Action object rather than returning a literal as I've been instructing. So, in this week's post, I plan to cover the exact steps I recommend going through to upgrade to NgRX 4, why I don't think instantiating an Action if the right way to create new actions, and some possible enhancements you can make once you've upgraded to the new version. <figure>![](/uploads/2017/08/2017-08-15.png "How to Upgrade NgRX to 4.x")<figcaption>Photo credit: [Sean MacEntee](//www.flickr.com/photos/smemon/6379436711/) via [VisualHunt.com](//visualhunt.com/re/6e8a99) / [ CC BY](//creativecommons.org/licenses/by/2.0/)</figcaption></figure>

<!-- more --> 

Upgrade NgRX to 4.x
-------------------

### TypeScript 2.4.x

As the upgrade mentions, you'll need to upgrade TypeScript to version 2.4.x and RxJS to 5.4.x before you can upgrade to NgRX 4.  So, the first thing we want to do is that we will want to upgrade to TypeScript 2.4.x.  In your package.json file, find the line that you are currently using to install TypeScript and make sure the version says

`"typescript": "^2.4.2",`

Then, delete the node_modules/typescript directory and run

`npm install`

to install the new version.  Since the Angular CLI uses the version that is local to the project, this will compile your code with TypeScript 2.4.x.  If you aren't using the CLI, I'm assuming you are smart enough to figure out the tweaks you have to make.

Next, you'll want to build your project.  You may find that the improved type checking in the 2.4 causes some compile errors you'll want to fix.  If you are linting with the --type-check option, you'll want to run your linter and fix any linting errors.

For good measure, you should run your application and make sure it still works.

### RxJS 5.4.x

Now that you have everything working with the new compiler, it is safe to upgrade RxJS.  Again, find the current "rxjs": "ve.rs.ion" line and change it to `"rxjs": "^5.4.3",` Delete the node_modules/rxjs directory and run `npm install` to install the new version of RxJS. Similarly, recompile, re-lint and run to verify that nothing has broken. To be clear, the reason we are making these upgrades so incrementally is so that when something doesn't work, we know what change caused it to not work.

### Upgrade NgRX/Store and NgRX/Effects

Remove @ngrx/core from package.json and change  the version number associated with @ngrx/store and @ngrx/effects.

`"@ngrx/store": "^4.0.2",`

`"@ngrx/effects": "^4.0.2",`

Delete node_modules/@ngrx and run `npm install` to install the new versions.

Now, the fun begins.

### Compose Has Moved

There is no longer an @ngrx/core.  That's why we removed the reference to it.  So, we need to change any references in our code that point to

`import {compose} from '@ngrx/core/compose';`

so, they now look like this:

`import {compose} from '@ngrx/store';`

### No payload on Action

One of the big structural changes in version 4 is that payload is no longer part of the Action interface.  I'll talk more about why I disagree with this change in a bit.  But for now, we just want to get your code running.  So, we are going to create our own Action interface.  Create an action.interface.ts file and place the following code in it:

`import {Action as NgRXAction} from '@ngrx/store';`

`export interface Action extends NgRxAction {`

`    payload?: any;`

`}`

If you aren't familiar with that import statement, the "as" is telling the compiler to import Action but we are going to refer to it AS NgRXAction in this file.  The rest is basic inheritance.

Now, everywhere you are currently importing Action from @ngrx/store, import this new version instead.  OK, we got payload back.  That will help a lot.

### Remove some code

Some of our code won't work right now.  So, we need to remove it.  If you are using StoreDevtoolsModeuls (probably in app-stores.module.ts or if you haven't been following along with how I code, it might still be in app.modules.ts.  Anyhow, you need to remove this.  Might as well remove it from package.json while you are at it. If you are using storeFreeze, you should remove that as well as the export function reducer() code that uses it.  By [using and enforcing linting, any benefit storeFreeze provided can be implemented by using the readonly-keyword and readonly-array lint rules](/typescript-functional-programming/). There may be places where you need to relax this rule simply because you are covering the mutation issue in another equally valid way.  For example, you may have a local array that you are returning from a function.  It is probably OK to mutate the array within the function because you just created it.  What we don't want to have happen is to mutate existing arrays. So, the function needs to be ReadonlyArray.

### reducers object

Next, change the const reducers object to be typed as an ActionReducerMap<object>.  Again, for those of you who may not have followed along with my previous post about using NgRX, we typically take the reducers object that is passed into provideStore() method in NgRX 2.x and make it a variable that we pass in rather than passing it in directly.  This makes maintaining that list easier. Speaking of provideStore(), that's been replaced with forRoot().  So, you should be able to just change the name on that one.  Oh, and since we removed the reducer() function, we should now pass in the reducers object instead.  If you never had a reducer() function, don't worry about it.  Just make sure you are passing in the ActionReducerMap<object> object.

effects
-------

In NgRX 2, we had to run() each effect we wanted to use.  In NgRX 4, we can pass them all into the EffectModule as an array: `EffectsModule.forRoot([     EffectsClass1, EffectsClass2, etc ]);`

### Compile

Yes, NOW you can finally try to compile, lint and run your code.  You may find that you'll need to fix up some of your tests in a similar way to how we've fixed the code so far. Assuming that everything works, this will allow you to continue on using NgRX 4 as though you were using NgRX 2.  But, you now have some options you didn't have before.

Feature Loading
---------------

The major advantage to upgrading to NgRX 4 is that we can now place our reducers, actions, effects closer to the code that will use them.  In fact, you can pass in an empty array in the forRoot() of both StoreModule and EffectsModule.  To load them with the module that will need them, use StoreModule.forFeature() and EffectsModule.forFeature().

payload Type Checking
---------------------

If you follow the instructions for upgrading on the main site, you'll see that the way they handle payload type checking is by creating an Action class for each action.  The payload gets passed into the constructor when you create the object and gets assigned to whatever variable you decide to use.  Since the parameter is typed, you automatically get type checking.  While this method works, there is a significant issue that isn't being discussed.  It takes significantly longer to new up an object in JavaScript than it does to use an object literal as we've been doing on our sites.  Grant it, no one will probably notice, but since we will be creating a lot of object during the course of the application, this is something to consider. I also find the syntax to use this method awkward. store.dispatch(new actionGroup.actionClass(payloadThing)); instead of store.dispatch(ActionClass.actionMethod(payloadThing)); It may not be obvious in the code above, but "actionGroup" is only what we named the group when we imported the action classes that actionClass() is a part of.  Something the compiler won't pick up on if you decide to change the name. Either way at this point in the game, we have options.  You can use the official way, or you can use my way.  What I would do instead is that I would create an action interface for each reducer.  If you've been using NgRX for a while, you'll already know that the payload is generally always the same, but occasionally varies.  For example, you might have a get() and delete() method that take an ID as a parameter, but your save() and update() methods would take structures.  Your action interface could end up looking like this: `import {Action} from '@ngrx/store';` `export interface thingAction extends Action {` `    id?: number;` `    payload?: ThingModel` `}` Then, when you were using get() or delete() you would look at "id" and if you were using update() or add() you would use payload.  You still get strong typing. But your code is much more efficient, more object oriented, and the compiler takes care of naming changes in a way the official way does not.
