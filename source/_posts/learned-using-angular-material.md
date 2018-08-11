---
title: What I Learned Using Angular Material
tags:
  - angular
  - angular material
url: 4488.html
id: 4488
comments: false
categories:
  - Angular
date: 2017-10-31 06:30:06
---

This past week I took Angular Material for a spin.  In the process, I learned a few things you might find helpful.  Some may be helpful even if you aren't interested in using Angular Material for your projects. <figure>![](/uploads/2017/10/2017-10-31.png "What I Learned Using Angular Material")<figcaption>Photo credit: [wuestenigel](//www.flickr.com/photos/30478819@N08/36098571263/) via [VisualHunt.com](//visualhunt.com/re/3c147a) / [ CC BY](//creativecommons.org/licenses/by/2.0/)</figcaption></figure>

<!-- more --> 

Choose Wisely
-------------

Up until recently, I've been reluctant to try Angular Material for two basic reasons.  First, it didn't seem quite ready for prime time.  I found out that it now has all of the components I would need for most of the applications I would want to develop.  Second, using Angular Material is a commitment.  Instead of writing primarily HTML, you write primarily angular material tags.  I thought this was a problem.  That I wouldn't have the flexibility to switch between Angular Material and some other CSS framework.  But, as it turns out.  Angular Material doesn't really lock me in any more than any other CSS framework or component library.  Which leads to the first lesson learned.  Choose your CSS frameworks and component library wisely.  If you decide to make a change mid-course, it is likely to suck up a lot of time from your project.  I would recommend doing some proof of concepts prior to committing to any component library or CSS framework.

Less Filling
------------

Because Angular Material is both a CSS framework and a component library, it turns out that writing templates using Angular Material is significantly terser than say, Bootstrap.  I don't mean to imply that it produces less HTML.  Just that writing the templates tends to be a bit cleaner overall.

@import
-------

I've written before about [how to include CSS in your Angular project](https://medium.com/@davembush/adding-css-and-javascript-to-an-angular-cli-project-2b843a8283f3), but I've learned a new trick while implementing Angular Material. 

This might be a relative new thing with the Angular CLI, but apparently, you can `@import` local and external CSS in your styles.css file.  When I evaluated the code that gets built, I found that the build process knows to keep the external code external yet bundles the internal code with the project.  This is something you could use regardless of what CSS framework or component library you are using.

Layout
------

As I suspected, a lot of layout expects you to use Flex CSS.  Since I already had decided to use Flex-Layout to manage my layout with Bootstrap, this was not a big surprise.  The Flex-Layout project started its life as part of the Angular JS Material project and split out as a separate project when both moved to support Angular.  At times, figuring out where to use what directive may require you to use Developer Tools.  But I didn't find this to be any more of a deterrent than using CSS in any other platform.  Just keep in mind that some of the Flex CSS is already defined in the Material components.

Creating Components from @Injectables
-------------------------------------

While creating my demo project, there were two places where I needed to create "snack bars" or dialogs.  In Angular Material, the way you accomplish this is by calling a method on an @Injectable.  aka, a Service.  This is interesting to me and I'm going to have to dig deeper to find out exactly how this works.

Testing
-------

The first time you write unit tests, you'll get a warning that says, "Could not find Angular Material core theme...".  To remove this error, you'll need to open karma.conf.js and add a "files:" section right under the "plugins:" section.

``` javascript
files: [{ 
  pattern: './node_modules/@angular/material/prebuilt-themes/indigo-pink.css', 
  included: true, 
  watched: true 
}],
```

You can add whatever pre-built theme you'd like.  I've added indigo-pink here.

Common declare/import/export
----------------------------

Strictly speaking, this isn't Angular Material specific.  But it was because of Angular Material that I discovered this trick.  Again, because it isn't Angular Material specific, you can use this in any of your Angular code that it makes sense. 

Angular Material requires you to import each of the component modules individually.  This allows tree shaking to be more effective.  Rather than put this code in `app.module.ts`, I created a `material-design.module.ts` file and imported it into my `app.module.ts`. 

Now, if you are familiar with how imports work, you'll already know that the components that are part of a module you import are only available to the parent module's components or below.  But we want the component to be available to any module that imports `material-design.module.ts`.  To do that, we need to export the same list of modules we imported. 

That sounded like too much repeating myself.  In an effort to make my code DRYer, I created a read only array with all my Material modules in it and then used the spread operator to include those in my imports and exports statement.

``` typescript
const materialDesignComponents: ReadonlyArray<Type<any>> = [
  MatSidenavModule,
  MatToolbarModule,
  MatListModule,
...
];

@NgModule({
  imports: [
    CommonModule,
    ...materialDesignComponents
  ],
  exports: [
    ...materialDesignComponents
  ],
  declarations: []
})
export class MaterialDesignModule { }
```

This saved me a lot of time, not just in typing, but in tracking down bugs because I simply forgot to include the modules in both places.

Sample Project
--------------

If you are interested, you can find [my sample project on GitHub](//github.com/DaveMBush/angular-material-demo).
