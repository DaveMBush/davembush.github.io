---
title: How to Avoid Binding to Computed Values in Angular
date: 2021-05-08 11:55:53
tags:
  - angular
  - ngrx
  - functional
  - reactive
  - mvp
---

In my previous article about [Angular Performance](../optimizing-angular-for-speed/), I indicated that one of the many ways you can produce an Angular application that performs well is by ensuring that you bind to a computed value.

First, what did I mean by that?

<!-- more -->

There three ways you might bind to a computed value.

The most obvious way is by calling a method in the template's corresponding class. But, properties count to. To be clear, a property is some code that uses a `get` prefix.

``` typescript
get somePropertyNameHere() {
  return 'something here';
}
```

And the least obvious way of all is by assigning an inline object to a property.

``` html
<app-component [someProp]="{ foo: classMember }">
```

Every time this code renders, it creates a new object, regardless of where `classMember` comes from. `classMember` could be a field, a property or a method.  It won't matter.

You particularly need to watch out for this when you are using `ngClass` or `ngStyle`.

Now, you might think you need to avoid this when you use the square brackets to assign a variable to a component property, but this also goes for ngIf and ngFor code as well.

What we are striving for is to not have the template recompute anything unless the data has actually changed.

The only exception to this is the ngFor trackBy attribute which is used to ensure the whole list doesn't re-render just because the array changed. I'm assuming you are using this.

But, you say, I NEED to compute those values in order to display them correctly.

OK, let's assume for a second that this is true. The way you do this is with pipes.

## Pipes

You should already be familiar with pipes because this is how we format dates, format numbers and subscribe to observables in the template.  But you aren't limited to using the built-in pipes. You can create your own, and it is quite easy to do.

All you need is a class with the `@pipe` attribute that implements the `PipeTransform` interface.

The code you would normally place in the method of property of your component class can go in here instead.

By doing this, Angular can see that the variable that is passed to it has changed, or not, and can determine when the code needs to be recomputed.

## onChange()

Another way you might achieve the same goal is by trapping the `onChange()` event for the component.

Many times, any value that we would compute in a method or property to be pulled from the template is only going to change if an `@input` value changed.

Instead of pulling the value and computing it on every change detection cycle, you can listen for the dependency during the onChange event and assign the new value to a class field and have your template look at the field instead.

Be careful though. If you are lazy and recompute the field every time `onChange()` is called, you really haven't gained anything by it.  `onChange()` will pass into you exactly what changed and you can use that to determine if the value should be recomputed.

## NgRX Store Selector

My favorite means of achieving the goal of not calling computed values is by using NgRX Store Selectors.

When you are new to NgRX you may only be using the simple selector.

``` typescript
let observerThing = this.store.pipe(select(x => x.storeSlice));
```

And of course you can drill down from storeSlice into children if you need to.

If you've graduated to Feature Selectors, your code may have graduated to:

``` typescript
let observerThing = this.store.pipe(select(featureSelector));
```

where `featureSelector` looks something like this:

``` typescript
createSelector(
  createFeatureSelector('feature'),
  x => x.featureSlice);
```

but there are other uses for `createSelector`.

You can use it to combine slices:

``` typescript
createSelector(
  slice1Selector,
  slice2Selector,
  (s1, s2) => combineLogicGoesHere
);
```

Or, you can use it to create a selector for a specific bit of information.

``` typescript
createSelector(
  sliceSelector,
  s1 => s1.s1Part
);
```

And by combining this, we can create a selector that does our formatting for us only when the underlying data has changed.

``` typescript
let partSelector = createSelector(
  sliceSelector,
  s1 => s1.s1Part);

let formatedPart = createSelector(
  partSelector,
  ps => formatPsHere(ps)
);
```

And now, our template can listen to this the same as it would listen to any other selector.

This is the method I most prefer. By pushing this code down into a selector, I don't have to be concerned with explicitly detecting what has changed. I let the selector do that for me. I just respond to the change when it happens.
