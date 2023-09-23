---
title: Optimizing Angular For Speed
date: 2023-09-23 08:34:41
tags:
  - angular
  - performance
  - best practice
---

I was recently asked how I would optimize an Angular site for speed. Interestingly, I've never written about this explicitly even though I've done a lot of work related to this issue.

For the purposes of this article, I'm going to assume you have already implemented most of the things that Angular gives you "for free." For example, we won't discuss "Tree Shaking" or "AOT." Those are given.

Here are my 30 or so tips on how to optimize an Angular application for speed.

<!-- more -->
## Change Detection

### OnPush Notification

Much has been written about how Angular Change Detection works, so I'm not going to go into much detail about this other than reiterate the basics.

Without OnPush notification, angular will check the component to see if anything changes whenever an event occurs. This means something as simple as a mouse move will cause change detection evaluation.

With OnPush, change detection on a component will happen only if one of the component's properties (field marked with @Input() attribute) has changed or if you've explicitly told Angular that a change has occurred.

In my experience, this isn't going to generate enough of a performance difference that you can measure in seconds. But it will make a difference overall and is generally a quick win. So, start here.

One quick way to ensure all your future components use OnPush when using the CLI to generate components is to ensure you add the OnPush configuration to your Angular.json file.

You can find a good article about this [here](https://indepth.dev/overriding-angular-schematics/).

### Small Components

OnPush notification is only going to be useful if your components are small. Remember that once change detection has determined the component is dirty and needs to be re-rendered, the WHOLE component will be re-calculated even though there is no Virtual DOM diffing in IVY.

### Run Outside Zones

It helps to understand how change detection works in Angular to optimize Angular change detection. Part of that equation is that it hooks into the fired events. This is how, for example, Angular knows it should update your screen when a click event occurs or when data is returned from an HttpClient request.

But sometimes, we may trigger that event in our code when we know that change detection is unnecessary.

For example, I have code that runs every 20 seconds to see if we should automatically log the user out. To keep this from running the change detection logic, I run this outside of the Zones logic.

The specific API you are looking for, is `Zones.runOutsideAngular()`

Another place to look out for is the time-based RxJS operators such as `debounceTime()`, `throttleTime()`, etc. These operators will trigger change detection every time they fire. If you are using them in your code, consider running them outside of the Zones logic or use unpatched versions of the operators.

### Turn Off Zones

Change detection might get triggered in multiple ways. And, in fact, your app probably doesn't need all of them.

You can turn things off using `__Zone_ignore_on_properties`.

There is more information on this [here](https://github.com/angular/zone.js/blob/master/STANDARD-APIS.md).

That being said, I've done some performance testing and it turns out that [Change Detection is not the first place to look if your screen is rendering slowly](../ngzones-performance-impact/).

### Use Pipes

This tip is only really valuable in the cases where the Functional programming's pure functions is relevant.

The idea behind pure functions, at least as it applies to pipes, is that a pure function will always return the same value when given the same parameters. So, for example, the function `Add(a, b)` will always return 4 given the parameters 2 and 2.

Why recompute the value when you give it the same parameters as the previous time you called it?

Using memoization, you can further take advantage of this optimization on functions that take extra time. This is what Angular Pipes do for you under the hood. But by implementing it yourself, you can cache more than the last value. I'm talking about the practice of holding onto a computed value and using the parameters as a key to look up the return value. Thus avoiding the computation completely.

Use this optimization tip intelligently, though. Sometimes, values might be "the same," but the content is different. I'm thinking about times when you would mutate the contents of an object. Say an array you added a value to instead of creating a new array. The array object pointer is still the same, so your pure function will not recompute the return value because the pointer didn't change.

If OnPush isn't giving you all the optimization that you are looking for, creating a pure Pipe is the next step along the same lines.

However, this is one place where I'd spend the extra time to verify that my "optimization" isn't making the performance worse.

### Use NgRX Selectors

Many people aren't aware that NgRX has a Selector mechanism allowing memoization. When you call the Selector,  if nothing it depends on has changed, you get back the same answer you got the previous time you executed the method without having to run all the code that gave you the answer the first time.

It also looks a lot cleaner than the `() => state.subState` mechanism we started out with.

Note that parameterized selectors can't take advantage of this memoization unless you roll your own.

``` typescript
const selectMemoizedThing = createSelector(...);

// vs

const selectCantBeMemoized = (param) =>
  createSelector(... uses param in here...)
```

### Don't Bind to Computed Values

If your value is computed, Angular can't easily determine if it has changed unless it calculates it.

There are three ways to solve this issue.

The first and most popular is to use a pipe, as described above. But it is the least desirable because it still forces the change detector to do more work than necessary.

The second way to achieve this goal is by assigning a member field the computed value during the component's `ngOnChanges` event. Do this when the computed value uses values all internal to the component.

Third, you can perform ALL the calculations in a Selector so that the data you need is already available and is easy to detect by the time you are binding.

Finally, if you can, do the computation on the server so it isn't even an issue on the client.

For more information on this topic see my article [How to Avoid Binding to Computed Values in Angular](../how-to-avoid-binding-to-computed-values-in-angular/)

### NgRX Features

And while we are on the subject of NgRX, instead of creating one monster state to rule them all in the AppModule, use the Features to only instantiate the store reducers, effects, etc., as you need them.

### Use RxJS distinct()

Did you know that your component is probably re-rendering more times than needed? This is because `@Input()` values will mark the component every time it thinks it might have received a new value. You can prevent this by using `distinctUntilChanged()` on the Observable.

### Use RxJX replay()

Another trick you can use is to use `replay()` on the Observable. There are various ways to configure this, but the advantage is that this is another way to prevent calculations you don't need. If the value isn't going to change as you render the content multiple times on the screen, you only need to compute once and allow `replay()` to memoize the return.

### Upgrade to IVY

In general, I suggest that you always keep your Angular version up to date.

While we are on the topic of upgrades, in an enterprise environment, I suggest never to fall more than 2 major versions behind and never use the current major version. I keep my team using the latest version of the major version before the current version. This keeps us relatively current but far enough behind that someone else has figured out all the quirks of the version we are using.

At a minimum, ensure you use a version that uses the IVY rendering engine to take advantage of the performance gains it introduced.

## Be smart about HTML and CSS

### Style the Host of your Components

Another place you can eak out a bit of performance without a lot of work is by eliminating the wrapping DIV element from your components.

Most Angular developers are unaware that when the HTML template is rendered, the tag you use to render the component is also rendered.  So if you have a component who's tag is \<foo\> and inside that component you included a DIV element that wraps all your control's content, what actually renders on the screen is

``` html
<foo>
  <div>
    html for component here
  </div>
</foo>
```

Now, it is true that the \<foo\> will get ignored by the browser. But it still has to evaluate it and determine that it shouldn't do anything with it and it is extra context that has to be packaged by webpack.

By styling \<foo\> to look like a DIV element by using

``` css
:host {
  display: block;
  width: 100%;
  ... whatever other styling you
  might have given your div tag here
},
```

we can eliminate the inner DIV from our component and gain a bit of performance without going to a lot of trouble. The amount of performance gain you will see will depends on how many child components are being displayed at any one time.

### Don't overuse SASS/SCSS

My general advice is to use CSS for your application and SASS for your component library or theme. Although, even in your component library, most of what you need to do can be done easily with CSS.

I admit. When LESS and SASS first came out, I saw all the advantages. Since then, I've been persuaded by three events.

First, if you write your Angular components small enough and use view encapsulation, you shouldn't need any of the features that SASS would provide.

Second, I've seen SASS abused more often than I've seen it used well. One project I worked on had 200 character class names because one of the developers was using & to concatenate the class names. The template code was cluttered with these long class names, making it difficult to read. Please! Don't do this to your future self!

Third, CSS has grown up. CSS variables now do most of what we may have needed to use SASS for. And, if you can keep your font and color information strictly in a theme.css file, you should never need to access them from your application components.

Simple CSS is performant CSS.

### ng-content Bad Practices

I've only recently realized that some of you are doing some pretty strange things with `ng-content``, and then you wonder why your application is slow.

As a refresher, `ng-content`` is what you use when you want to use content projection to pass content into a component. For example, if you wanted to pass in a list of items to a component, you might do something like this:

``` html
<my-component>
  <div *ngFor="let item of items">
    {{item}}
  </div>
</my-component>
```

Now, the first thing you should ask yourself here is, "Do I need to use content projection here?"

We've found a significant performance penalty for using content projection. So, if you can avoid it, you should.

Notice, I said avoid. Not eliminate. There are some valid reasons for using content projections. For example, low-level components often need to use content projection because they could be a container for displaying anything.

But, if you are using content projection to display exactly the same content or choice of content every time, consider eliminating the content projection and just hard-coding the content into the component. This will make the code easier to understand, and it will make the content render faster.

The other crazy implementation I've seen recently is using ngIf inside ng-content.

``` html
<ng-content *ngIf="foobar">
</ng-content>
```

It may not be obvious, but you should evaluate your DOM when the page is rendered with foobar equal to false. What you'll see may surprise you. Inside of ng-content will be all the elements that are projected into the ng-content DOM element, but they will all be hidden.

So what? So, they will all have to be evaluated by the browser even though they aren't visible. This is a waste of resources and time rendering.

### SVG references vs SVG inlining

Another similar, hidden performance issue is the use of SVG Inlining. When you inject an SVG image into the page using inlining, the browser must evaluate the DOM elements. If you inline the same SVG image multiple times, the browser has to evaluate the DOM elements multiple times.

The cure is to inline the SVG images once, and then, when you need to display the image, you can reference it using the SVG reference mechanism. There are multiple ways you might do this. One is to load them up front using a sprite and inject the sprite into the DOM. Another would be to inject the SVG image into the DOM using JavaScript the first time it is used and then reference it from that point forward.

### Measure your CSS

The most overlooked area of performance I know of is measuring CSS. I can't go into a lot of detail about this here because it would take far too much time to explain (and read about) but there are good articles on the internet that discuss measuring browser performance using flame charts and other mechanisms that are part of your browser's developer tools.

## Compile for Production

Another place where we shouldn't have to mention that it is even an option is compiling for production. 

In one product I worked on, I discovered we were deploying code using the development environment instead of the production environment. I only discovered this because we were getting the change detection after change warning in production. That's the most notable difference. But this check happens with every change detection cycle and further slows down the code.

## Cache your code on the browser

If you build your application with an option that attaches a hash code to the file name, you can cache your javascript code on the browser so that the next time that browser tries to access the code, it will load it from the cache.

Do not cache your index.html file, as you need to make sure that file always gets downloaded.

Specifically, the options you want to implement on your server for your static files are:

- immutable
- max-age 365

This will prevent the browser from even checking the server for new code.

## CDN

Similarly, putting your files closer to the people retrieving them will improve your performance around "time to first render."

## Perception Beats Reality

One place that gets overlooked when discussing performance is the "time to first render."

We recently ran into a situation where the code took so long to load that the client didn't even see the spinner we had put in place to indicate we were loading data.

To fix this, we added spinner code directly into our html file. It takes slightly longer to load the index.html file but displays more quickly than if we waited for our javascript to load.

## TrackBy X in ngFor

Another way to eak out more performance is by implementing the `trackBy` property in your `ngFor` loops.

This is particularly useful when displaying a list that updates multiple times from the server. Because it is a brand-new object, every refresh will update the whole list.

By using trackBy, you can tell Angular only to update the elements in the list that have changed by whatever you specify in TrackBy.


## Lazy Loading

### Lazy Load Routes

Most applications have multiple routes that they access. But even if yours doesn't, you should set your code up to use Lazy Loading for three reasons.

First, small apps grow up. Eventually, you will have multiple routes. If your app is so small that it doesn't, implementing Lazy Loading of routes isn't going to hurt anything.

Second, implementing Lazy Loading will break your code down further into multiple files. Most of the code in those files will never change, so once they are loaded and cached as instructed above, the only code that will ever need to be downloaded is code in your route when it changes.

Third, having more files to download could help in the initial load of your application, as the client now has more opportunities to download the files in parallel. Measure this! I've seen this overused to the point of causing worse performance!

### Pre-load Lazy Loaded Routes

Once you've started your app, you should load all the other routes in the background so your customer doesn't have to wait for the next page to render.

This is even more important when your application is on a mobile device that doesn't have an Internet connection at times.

You can also be selective about which routes you pre-load.

### Lazy Loading Resources

Angular does most of this for you. Especially as it concerns CSS, but if your application has a lot of images, you should consider lazy loading those images.

### Virtual Scrolling

There are multiple implementations of this. But the idea is only to render what the user can see as the user scrolls rather than rendering everything into the DOM and then letting the browser to the scrolling.

I reduced the rendering time from 8 seconds to 1 second on one mega app I worked on by implementing this concept.

## Good Architecture

### General Patterns and Practices

I've worked on optimizing an application where one of the first things we did was to straighten out the basic architecture.
I often tell people, "If you had any idea how software gets created, you'd be amazed that anything works as well as it does." This application was no different.

Someone had wanted to implement a feature in the next version of NgRX, so they copied code from the next version into the version we were using.

It sounds harmless. But the result was the NgRX change detection got thrown into a loop that took thousands of cycles to resolve instead of just one or two. This, in turn, slowed the rendering time down on the application to a point where just switching routes took multiple seconds. On the order of 5–10 seconds.

Upgrading the application to the version we wanted to use solved the problem.

Other similar issues with this code were solved simply by fixing how the code had been assembled.

### Avoid Code That Changes Angular

Similar to the issue above is code that adds directives or other functionality that changes how Angular fundamentally works.

There are two major reasons for this. The first is that we always want to be able to enlist people to help us with our code. Most of the time, we are trying to hire good Angular developers. If our code no longer looks like Angular, why hire Angular developers?

Second, if the code changes how Angular works, it is probably hooking into some Angular internal that could change in a subsequent version of Angular. This means that you will have to wait for the code's author to update their code before you can upgrade your application.

Third, you can't be sure some little project that thought it would be cool to add some feature to Angular to get around some perceived issue has fully tested their code. You could introduce a bug into your application that you must track down.

Finally, you have to ask yourself. Does this feature, or set of features, solve a problem that would still exist if I'd written my code correctly in the first place? The one package I have in mind as I write this has contributed more to the technical debt on my project than any other package or programmer we've had interact with our code.

Be careful what code you allow into your application; it could cost you far more than it is worth in the long run.

## Data Access

So far, we've talked about what to do once we've retrieved the data into our application, but many performance issues can be avoided simply by paying attention to how and when we load our data from the server.

### Only Load What You Need

One guiding principle for data access is that you should only load the data you need from the server at the point you need it.

An example of where you might see this performance issue is that we tend to do a massive join of data on the server and return everything we retrieved, even if we don't need the data for the operation at hand.

The less data you access, the faster your application will perform.

### Normalization is a Curse

One way to send less data back to the server is to send it back in raw form rather than nested and normalized.

For this, I recommend a product called Normalizr. This takes the nested data and breaks it into multiple tables. The product was originally written for client-side code to make using Redux and NgRX easier. But there are now server-side implementations you can use that structure the data before sending it back so that you only send back one instance of the data you need rather than multiple instances because multiple parent rows use it.

Better than using Normalizr, which is now a bit dated, you should consider only asking for un-normalized data from the server and dumping it into NgRX entities. You can use NgRX selectors to join the data together however needed. Now that everyone should at least be using HTTP2, the cost of simultaneously making multiple requests to the server is negligible.

### Pre-load Lookup Tables

Along with this same idea. We often have nested data that is the result of some lookup table. By using Normalizr, you could load those tables up front and then only return the key to those tables and use Normalizr and NgRX Selectors to do the JOIN on the client side, further reducing the overall amount of data that you need to retrieve at any one time.

### Virtual Arrays

I mentioned Virtual Scrolling above as one way to reduce the rendering time. By combining this with a concept I refer to as Virtual Arrays, you can similarly only retrieve the data the user can see, further reducing the perceived time to render.

A Virtual Array is an object that looks like an array from an API perspective, but the implementation uses the database as the storage location. You can either retrieve the data from the database whenever it is requested from the array or memoize it so that it is only retrieved once.

## Memory Management

When most people think of performance, they immediately think of "how to impact the performance of good code." But the most overlooked performance issue in JavaScript generally is memory. Specifically memory leaks.

There are now several tools available for detecting and tracking down memory leaks. Find one and use it.

## Micro Tweaks

### Service Workers

Similar in concept to the virtual arrays above but a standard API that you can implement globally. You can use service workers to cache data from the server and only ask the server for it again after you consider it stale.

You can also use this to cache the index.html file locally, so you don't have to retrieve it until there is a new version.

### WebAssembly

I've yet to find my code is so slow that this actually makes sense, but it is theoretically possible that you'd find even more performance by creating services using WebAssembly. This would mean you'd need to write the code in something other than TypeScript and Angularize it by wrapping it in TypeScript/Angular code. But I've seen at least one article on the web where they did just that.

### WebWorkers and Shared Workers

This is another place where there is at least a theoretical possibility that it would provide additional performance. At the very least, it can provide the appearance of performance.

Some places to consider using Shared Workers specifically are with Web Sockets and Data Retrieval. By placing Web Socket listeners in a Shared Worker, you create one connection to the server. The Shared Worker can then relay those messages to the client code on as many windows as are currently open.

Similarly, by using a Shared Worker as the main place that retrieves your data, you can cache the data and rebroadcast the new data to all the open browser windows listening to the Shared Worker.

## Server Side Considerations

### Normalization is a Curse Here Too

I once worked on an application where the data was being retrieved from 20 tables joined together. When we looked at what was going on, we realized that most of that data was static when the client was using it and that we could dump most of the join results into a flat table and use it instead. This was another case where we went from an 8-second load time to 1 second.

The performance considerations around JOINs vary depending on what database you are using. But it is something to keep in mind all the same.

### Server Side Caching

In many systems similar to the one I mentioned above, most of our calls to the server could be considered Functionally pure enough that we could cache the data.

I [did some timings on this several years ago](/what’s-the-truth-about-running-asp-net-webapi-asynchronously/) and the results, even caching for a second, gave some pretty impressive results.

For example, those lookup tables I discussed earlier don't need to be retrieved from the database every time we make a call for them. We should be able to make a call for the data once and then return that data every time it is requested until the data is refreshed.

## Conclusion

There are probably other ways to optimize your application that I've forgotten or, more likely, do out of habit now.

What I've written about above are the ones that stand out as the often overlooked issues.
