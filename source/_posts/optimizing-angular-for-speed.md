---
title: Optimizing Angular For Speed
date: 2022-04-23 08:34:41
tags:
  - angular
  - performance
  - best practice
---

I was recently asked how I would optimize an Angular site for speed. Interestingly, I've never written about this explicitly even though I've actually done quite a bit of work related to this issue.

For the purposes of this article, I'm going to assume you have already implemented most of the things that Angular gives you "for free." For example, we won't talk about "Tree Shaking", that's a given.

So, I give you my 30 or so tips on how to optimize an Angular application for speed.

<!-- more -->
## Change Detection

### OnPush Notification

Much has been written about how Angular Change Detection works so I'm not going to go into a lot of detail about this other than to reiterate the basics.

Without OnPush notification, angular will check the component to see if anything changed every time an event occurs.  This means something as simple as a mouse move will cause change detection evaluation.

With OnPush, change detection on a component will happen only if one of the properties (field marked with @Input() attribute) of the component has changed or if you've explicitly told Angular that a change has occurred.

In my experience, this isn't going to generate enough of a performance difference that you can measure in seconds.  But it is going to make a difference overall and it is a generally quick win. So, start here.

One quick way to make sure all your future components use OnPush when using the CLI to generate components is to make sure you add in the OnPush configuration in your Angular.json file.

You can find a good article about this [here](https://indepth.dev/overriding-angular-schematics/).

### Small Components

OnPush notification is only going to be useful if your components are small. Keep in mind that once change detection has determined the component is dirty and needs to be re-rendered, the WHOLE component will be re-rendered.

### Run Outside Zones

To optimize Angular change detection, it helps to understand how change detection works in Angular. Part of that equation is that it hooks into the events that are fired.  This is how, for example, Angular knows to update your screen when a click even occurs, or when data is returned from an HttpClient request.

But sometimes we may trigger that event in our code when we know that change detection is not required.

As an example, I have code that runs every 20 seconds to see if we should automatically log the user out. To keep this from running the change detection logic, I run this outside of the Zones logic.

The specific API you are looking for, is `Zones.runOutsideAngular()`

### Turn Off Zones

As I said above, there are multiple ways change detection might get triggered.  And, in fact, your app probably doesn't need all of them.

You can turn things off using `__Zone_ignore_on_properties`.

There is more information on this [here](https://github.com/angular/zone.js/blob/master/STANDARD-APIS.md).

That being said, I've done some performance testing and it turns out that [Change Detection is not the first place to look if your screen is rendering slowly](../ngzones-performance-impact/).

### Use Pipes

This tip is only really valuable in the cases where the Functional programming's pure functions is relevant.

The idea behind pure functions, at least as it applies to pipes, is that a function that is pure will always return the same value when given the same parameters. So, for example, the function `Add(a, b)` will always return 4 given the parameters 2 and 2.

Why recompute the value when you give it the same parameters as the previous time you called it?

You can further take advantage of this optimization on functions that take extra time by using memoization. This is the practice of holding onto a computed value and using the parameters as a key to lookup the return value. Thus avoiding the computation completely.

Use this optimization tip intelligently though.  There are times when values might be "the same" but the content is different.  I'm thinking about times when you would mutate the contents of an object.  Say an array you added a value to instead of creating a new array.  The array object pointer is still the same and so your pure function will not recompute the return value because the pointer didn't change.

If OnPush isn't giving you all the optimization that you are looking for, creating a pure Pipe is the next step along the same lines.

This is one place where I'd spend the extra time to verify that my "optimization" isn't actually making the performance worse.

### Use NgRX Selectors

Many people aren't aware that NgRX has a Selector mechanism that allows for memoization.  This means when you call the Selector, if nothing it depends on has changed then you just get back the same answer you got the previous time you executed the method.

It also looks a lot cleaner than the `() => state.subState` mechanism we started out with.

### Don't Bind to Computed Values

If your value is computed, Angular can't easily determine if the value has changed, unless it calculates it.

There are three ways to solve this issue.

The first and most popular, is to use a pipe as described above.

The second way you could achieve this goal is by assigning a member field the computed value during the component's `ngOnChanges` event.

Finally, my preferred way, is to do ALL the calculations in my Selectors so that, by the time I'm binding, the data I already need is available and is easy to detect.

For more information on this topic see my article [How to Avoid Binding to Computed Values in Angular](../how-to-avoid-binding-to-computed-values-in-angular/)

### NgRX Features

And while we are on the subject of NgRX, instead of creating one monster state to rule them all in the AppModule, make use of the Features so that you are only instantiating the store reducers, effects, etc. as you need them.

### Use RxJS distinct()

Did you know that your component is probably re-rendering more times than it needs to?  This is because @Input() values will mark the component every time it thinks it might have received a new value. By using `distinct()` on the Observable, you can prevent this from happening.

### Use RxJX replay()

Another trick you can use is to use `replay()` on the Observable. There are various ways to configure this, but the advantage is this is yet another way of preventing calculations you don't really need. If the value isn't going to change as you render the content multiple times on the screen, you only need to compute once and allow `replay()` to memoize the return.

### Upgrade to IVY

In general, I would suggest that you always keep your Angular version up to date.

While we are on the topic of upgrades.  In an enterprise environment, my suggestion is to never fall more than 2 major versions behind and never us the current major version.  I try to keep my team using the latest version of the major version one major version prior to the current version.  This keeps us relatively current but far enough behind that someone else has figured out all the quirks of the version we are using.

At a minimum, make sure you are using a version that uses the IVY rendering engine to take advantage of the performance gains it introduced.

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

First, if you write your Angular components small enough and you use view encapsulation, you shouldn't need any of the features that SASS would provide to you.

Second, I've seen SASS abused more often than I've seen it used well. Once project I worked on ended up having 200 character class names because one of the developers was using & to concatenate the class names. The template code was cluttered with these long class names and it made the code difficult to read.

Third, CSS has grown up. CSS variables now do most of what we may have needed to use SASS for. And, if you can keep your font and color information strictly in a theme.css file, you should never need to access them from your application components.

## Compile for Production

Another place where we really shouldn't have to mention that it is even an option is the process of compiling for production. In one product I worked on I discovered that we were deploying code using the development environment instead of the production environment.  The only reason I discovered this is because we were getting the change detection after change warning in production.  That's the most notable difference. But, this check is happening with every change detection cycle and is further slowing down the code.

## Cache your code on the browser

If you build your application with an option that attaches a hash code to the file name, you can cache your javascript code on the browser so that the next time that browser tries to access the code it will load it from the cache.

Do not cache your index.html file as you need to make sure that code always gets download.

Specifically, the options you want to implement on your server for your static files are:

- immutable
- max-age 365

This will prevent the browser from even checking the server for new code.

## CDN

Similarly, putting your files closer to the people who will be retrieving them will go a long way to improving your performance around time to first render.

## Perception Beats Reality

One place that gets overlooked when we talk about performance is time to first display.

We recently ran into a situation where the code was taking so long to load that the client didn't even see the spinner we had put in place to indicate we were loading data.

To fix this we added spinner code directly into our html file.  It takes slightly longer to load the index.html file but it displays more quickly than if we also waited for our javascript to load.

## TrackBy X in ngFor

Another way to eek out a bit more performance is by implementing the TrackBy property in your ngFor loops.

This is particularly useful when you are displaying a list that updates multiple times from the server.  On every refresh, because it is a brand new object, it will update the whole list.

By using TrackBy, you can tell Angular to only update the elements in the list that have changed by whatever you specify in TrackBy.

Be careful with this. If you are retrieving data where the ID stays the same but the content for that ID changes, what you most certainly don't want to do is to TrackBy ID.

In this case you would either not use TrackBy or maybe use ID in combination with LastUpdate date.  I've seen this implemented incorrectly way too often.

## Lazy Loading

### Lazy Load Routes

Most applications have multiple routes that they access. But even if yours doesn't you should set your code up to use Lazy Loading for three reasons.

First, small apps grow up.  Eventually you will probably have multiple routes.  If your app is so small that it doesn't, implementing Lazy Loading of routes isn't going to hurt anything.

Second, implementing Lazy Loading will break your code down further into multiple files. Most of the code in those files will never change so that, once they are loaded and cached as instructed above, the only code that will ever need to be downloaded is code in your route when it changes.

Third, having more files to download could help in the initial load of your application as the client now has more opportunity to download the files in parallel.

### Pre-load Lazy Loaded Routes

Once you've started your app, you should load all the other routes in the background so your customer doesn't have to wait for the next page to render.

This is even more important when your application is on a mobile device that doesn't have an Internet connection at times.

You can also be selective about which routes you pre-load.  But I don't recommend it.

### Lazy Loading Resources

Angular does most of this for you.  Especially as it concerns CSS.  But, if your application has a lot of images, you should consider lazy loading those images.

### Virtual Scrolling

There are multiple implementations of this. But the idea is to only render what the user can see as the user scrolls rather than rendering everything into the DOM and then letting the browser to the scrolling.

On one mega app I worked on, I reduced the rendering time from 8 seconds to 1 second by implementing this concept.

## Good Architecture

I've worked on the optimization of an application where one of the first things we did was to straighten out the basic architecture.

I often tell people, "If you had any idea how software gets created, you'd be amazed that anything works as well as it does."  This application was no different.

Someone had wanted to implement a feature that was in the next version of NgRX and so, copied code from the next version into the version we were using.

It sounds harmless. But the result was the NgRX change detection got thrown into a loop that took thousands of cycles to resolve instead of just one or two.  This, in turn, slowed the rendering time down on the application to a point where just switching routes took multiple seconds.  On the order of 5 - 10 seconds.

Upgrading the application to the version we wanted to use solved the problem.

There were other similar issues with this code that were solved simply by fixing how the code had been assembled.

## Data Access

So far we've talked about what to do once we've retrieved the data into our application, but many performance issue can be avoided simply by paying attention to how and when we load our data from the server.

### Only Load What You Need

One guiding principle for data access is that you should only load the data you need from the server at the point you actually need it.

An example of where you might see this performance issue is that we tend to do a massive join of data on the server and return everything we retrieved even if we don't need the data for the operation at hand.

The less data you access, the faster your application will perform.

### Normalization is a Curse

One way to send less data back to the server is to send it back in its raw form rather than nested and normalized.

For this, I recommend a product called Normalizr.  This takes the nested data and breaks it into multiple tables. The product was originally written for client side code to make using Redux and NgRX easier.  But there are now server side implementations you can use that structure the data prior to sending it back so that you only send back one instance of the data you need rather than multiple instances because it is used by multiple parent rows.

Better than using Normalizr, which is now a bit dated, you should consider only asking for un-normalized data from the server and dumping it into NgRX entities. You can use NgRX selectors to join the data back together however you need it. Now that everyone should at least be using HTTP2, the cost of making multiple request to the server at the same time is negligible.

### Pre-load Lookup Tables

Along this same idea. Many times we have nested data that is the result from some lookup table. By using Normalizr, you could load those table up front and then only return the key into those tables and use Normalizr and NgRX Selectors to do the JOIN on the client side further reducing the overall amount of data that you need to retrieve at any one time.

### Virtual Arrays

I mentioned Virtual Scrolling above as one way to reduce the rendering time. By combining this with an concept I like to refer to as Virtual Arrays you can similarly only retrieve the data the user can see further reducing the perceived time to first render.

A Virtual Array is an object that looks like an array from an API perspective but the implementation is actually using the database as the storage location.  You can either retrieve the data from the database whenever it is requested from the array or you can memoize the data so that it is only retrieved once.

## Micro Tweaks

### Service Workers

Similar in concept to the virtual arrays above but a standard API that you can implement globally. You can use service workers to cache data from the server and only ask the server for it again after you consider it stale.

You can also use this to cache the index.html file locally so you don't even have to retrieve it until there is a new version.

### WebAssembly

I've yet to find my code is so slow that this actually makes sense but it is theoretically possible that you'd find even more performance by creating services using WebAssembly.  This would mean that you'd need to write the code in something other than TypeScript and Angularize it by wrapping it in TypeScript/Angular code.  But I've seen at least one article on the web where they did just that.

### WebWorkers and Shared Workers

This is another place where there is at least a theoretical possibility that it would provide additional performance. At the very least, it can provide the appearance of performance.

## Server Side Considerations

### Normalization is a Curse Here Too

I once worked on an application where the data was being retrieved from 20 tables joined together.  When we went to look at what was going on, we realized that most of that data is static during the time the client was using it and that we could dump most of the join results into a flat table and use it instead.  This was another case were we went from an 8 second load time to 1 second.

I know the performance considerations around JOINs varies depending on what database you are using.  But, it is something to keep in mind all the same.

### Server Side Caching

In many systems, similar to the one I mentioned above, most of our calls to the server could be considered Functionally pure enough that we could cache the data.

I [did some timings on this several years ago](/what’s-the-truth-about-running-asp-net-webapi-asynchronously/) and the results, even caching for a second gave some pretty impressive results.

For example, those lookup tables I discussed earlier don't need to be retrieved from the database every time we make a call for them.  We should be able to make a call for the data once and then return that data every time it is requested until the data is refreshed.

## Conclusion

There are probably other ways to optimize your application that I've forgotten or, more likely, just do out of habit now.

What I've written about above are the ones that stand out as the often overlooked issues.
