---
title: NgZones Performance Impact
date: 2022-05-28 19:03:40
tags:
  - angular
  - change-detection
  - performance
---

I've been working on this article for a month, off and on. When I started, I set out to demonstrate the performance impact of change detection in Angular. In particular, I wanted to demonstrate that turning zones off would have a significant impact on performance even if you followed all my other advice regarding performance optimization.

That is, if you had:
- Implement OnPush notification
- Keep your components small
- Use pure pipes
- Use NgRX Selectors
- Use distinctUntilChanged on your Observables
- Don't bind to computed values.
- User RxJS distinct()
- Use RxJS replay()
- Style the :host
- Use TrackBy x in ngFor
- Lazy-load routes
- Preload lazy-loaded routes
- Lazy-load resources
- Use virtual scrolling
- Don't use nested data in your store
- Pre-load lookup tables.
- Use virtual arrays
- Tune your server

Once you've done all those things, the only question left is "exactly how much time does the change detection cycle take if nothing has changed?"

## First Attempts

Assuming that change detection would be an issue, I set out to determine how much of a performance impact I would see with some pretty simple code.  So I created a component that updated a value when a button was clicked.

I then used the performance tools in developer tools to see just how long the update cycle would take.

At first I thought I was on to something, but it was actually the compiler kicking in on the first run. When I clicked the button a second time, the performance was not any different regardless of where I put the changes.

I thought maybe the difference was the number of @Inputs that were being watched, but even updating all those did not make any significant difference.  Even using prop drilling, if the component didn't actually use the value from the @Input() there was no difference in performance.

## How Much Time Does ChangeDetection Take?

So, today I set out to see just how much time change detection takes if nothing updates in the app.  Just trigger a change detection cycle.

That's easy, create a button that you can click nested in the inner most component.

To get a better sense of timing I also created a tree of components 1000 levels deep.

The entire click event where the change detection cycle occurs take about 20ms with the developer tools CPU performance set to 4x.

## Conclusion

If you are turning off zones as THE way of optimizing your performance you are masking architectural issues that should have been addressed first. Don't let anyone tell you otherwise. It just isn't true. Take it from someone who tried to prove it was.
