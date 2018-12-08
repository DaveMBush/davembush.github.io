---
title: Unit Testing Angular(2+) with JSDOM
tags:
  - jsdom
  - karma
  - unit test
url: 4254.html
id: 4254
categories:
  - Angular
date: 2017-04-04 06:30:00
---

Unit Testing Angular(2+) with JSDOM can be problematic unless you know the secret handshake that allows ZoneJS and JSDOM to coexist.

The great thing about Angular is that you can write Unit Tests from the presentation layer all the way down to calls to the server.  But up until now, you either ran those tests in a browser, which doesn’t work well in a CI system, or you used PhantomJS, which tends to be REALLY slow!  But there is a better way, and hopefully, by the time this post goes live, the patches needed to use JSDOM will be available.  If not, I’ll show you the hack that I’ve found works and the pull request I’m hoping will go live.

<figure>![](/uploads/2017/04/image.png "Unit Testing Angular(2+) with JSDOM")<figcaption>Photo credit: [Juanedc](//www.flickr.com/photos/juanedc/14896919066/) via [Visual Hunt](//visualhunt.com/re/cf947c) / [ CC BY](//creativecommons.org/licenses/by/2.0/)</figcaption></figure>

<!-- more -->

## Why JSDOM?

As I mentioned in the introduction, there are two problems that JSDOM fixes.

To run the Angular unit tests, you need to run them in a browser. The problem with this is that you would need to have a browser installed on your CI server to run them, if you run them at all. It can be done, but if you are working in an environment like the one I work in, it isn’t going to be easy.

The second choice is to use PhantomJS. Unfortunately, PhantomJS, while easy to install, runs slowly. For all but the most trivial of applications, this isn’t going to work well.

JSDOM, on the other hand, runs fast like a browser, and doesn’t have the problems that running it on a CI system has. This is because it is a headless browser that never renders. All it does is produce HTML. For unit tests, this is all we really care about. And because it is running inside of Node, it is running as fast as the V8 engine will let it. Making it theoretically faster than running the tests in Chrome. I say, “theoretically faster” because I have not tested this and the V8 engines in the most recent browser tends to be a bit ahead of the V8 engine used in the most recent version of Node.

## The Problem

Knowing this was possible caused me to give it a try. What I found was that I routinely crashed with the following error.

`Cannot set property onreadystatechange of [object Object] which has only a getter`

As I googled this, I found several fixes for JSDOM that would allow this to work, but I also discovered that JSDOM did not consider this an issue they needed to fix. And rightly so, why should they adapt just so it would work for Angular2? But, what in the Angular2 code would cause this problem. By doing a search for `onreadystatechange` in my node_modules directory, I discovered that ZoneJS was:

1.  Saving off the original definition of `onreadystatechange`
2.  Overriding the definition with a getter (only)
3.  Setting the definition back to the original

Which all works well if the original `onreadystatechange` has a definition.  But in the case of JSDOM, it doesn’t. Then, when they set the definition back, nothing happens and we keep the definition they created.

## Solution 1

The file in question is `property-descriptor.ts` under the `lib/browser` directory (the js version is in the file `zone.js` under the `dist` directory). Sticking with the TS file… of version 0.8.5, scroll down to line 64 and you’ll see that they retrieve the current definition but never verify that they actually got something back. But at line 80 they set it back to an empty object if it doesn’t exist.

The easy fix that seems to work for me, is to just change the new definition so that it works if that is the one that is left over after this function completes:

``` javascript
Object.defineProperty(XMLHttpRequest.prototype, 'onreadystatechange', {
    enumerable: true,
    configurable: true,
    get: function() {
      return this.orsc;
    },
    set: function(f) {
      this.orsc = f;
    }
  });
```

Because we’ve changed the getter from returning a hardcoded value, we also need to set `onreadystatechange`

``` javascript
const req = new XMLHttpRequest();
req.onreadystatechange = function(){}
const result = !!req.onreadystatechange;
```

The full fix looks like this:

``` javascript
function canPatchViaPropertyDescriptor() {
  if ((isBrowser || isMix) && !Object.getOwnPropertyDescriptor(HTMLElement.prototype, 'onclick') &&
      typeof Element !== 'undefined') {
    // WebKit https://bugs.webkit.org/show_bug.cgi?id=134364
    // IDL interface attributes are not configurable
    const desc = Object.getOwnPropertyDescriptor(Element.prototype, 'onclick');
    if (desc && !desc.configurable) return false;
  }

  const xhrDesc = Object.getOwnPropertyDescriptor(XMLHttpRequest.prototype, 'onreadystatechange');

  // add enumerable and configurable here because in opera
  // by default XMLHttpRequest.prototype.onreadystatechange is undefined
  // without adding enumerable and configurable will cause onreadystatechange
  // non-configurable
  Object.defineProperty(XMLHttpRequest.prototype, 'onreadystatechange', {
    enumerable: true,
    configurable: true,
    get: function() {
      return this.orsc;
    },
    set: function(f) {
      this.orsc = f;
    }
  });
  const req = new XMLHttpRequest();
  req.onreadystatechange = function(){}
  const result = !!req.onreadystatechange;
  // restore original desc
  Object.defineProperty(XMLHttpRequest.prototype, 'onreadystatechange', xhrDesc || {});
  return result;
};
```

## Solution 2

The [current pull request](//github.com/angular/zone.js/pull/711/commits/edc9d7a2145f9ddc4acbe6a49d1325d676c65429) adds a bit more code that I'm assuming is needed. I haven’t tested this, but I'm assuming it is a safer alternative than my hack.

## One Additional Gotcha!

Once I had this basic issue solved, I was able to run my suite of test except one. It turns out `element.innerText` doesn’t exist in JSDOM. There is a technical reason for this that I won’t discuss here other than to say it is, evidently, somehow dependent on the rendering engine, and since JSDOM has no rendering engine (remember, it just produces HTML) it can’t really implement `innerText`. So, I had to refactor my test to use `innerHTML` instead. Trivial issue. Just something you need to be aware of.

## Setting Up Karma

Now, from here to the end, we are going to assume that this got fixed, or you are using one of the solutions above. Now, how do we set karma up to use JSDOM instead of Chrome or PhantomJS?

Well, for starters, you’ll need to `npm install --save-dev jsdom karma-jsdom-launcher`.

Then, you’ll need to make a few changes to your `karma.conf.js` file.

First, in the plugins array, add `require('karma-jsdom-launcher')`.

Then, at the bottom of the file, change the browsers line to specify `'jsdom'` instead of `'Chrome'`.

I normally just comment out the Chrome line and put in a line for jsdom so I can use Chrome to debug when I need to.

I’ve always said that Angular mixes the best of AngularJS and React and with this fix, we now have some React Unit Testing goodness added into the mix.
