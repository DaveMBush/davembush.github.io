---
title: VMWare's Clarity Design System for Angular
tags:
  - angular
  - clarity
  - css
  - ui
  - ux
url: 4581.html
id: 4581
comments: false
categories:
  - Angular
date: 2018-03-20 06:30:51
---

Unless you are a CSS wizard, you are probably using one of two CSS frameworks for your Angular projects or some sort of adaptation of them.  Bootstrap or Angular Material.  These have served us well, but they have one major flaw.  They target the "Mobile First" method of design.  This is great if your application must work on a mobile device.  But most corporate web applications target web applications. Have you ever heard any of these objections from your end users?

* Why is everything so big?
* Why can't I have the label NEXT to the input field?

And then you explain, it is so the screen can run on a mobile device and you hear, "But, this application will never run on a mobile device!"  Which is a valid point. Therefore, I was so excited to hear that VMWare has finally taken up the challenge of creating a Desktop First CSS Framework called [Clarity](//vmware.github.io/clarity/). 

<figure style="text-align: center">
{% asset_img 2018-03-20.jpg VMWare&#39;s Clarity Design System for Angular %}<figcaption>Photo credit: [Sean Hering Photography](//visualhunt.com/author/2edd3b) on [Visual Hunt](//visualhunt.com/re/764a8a) / [ CC BY](//creativecommons.org/licenses/by/2.0/)</figcaption>
 </figure>

<!-- more --> 

<!-- more -->

What Is Clarity?
----------------

To call it a CSS Framework though doesn't really do it justice.  It is a UI/UX System.  Much like Angular Material.  In fact, an application written using Clarity is going to end up looking a lot like a Material Design application.  The main differences are going to be, it will be built for the desktop rather than a mobile device.  This means the components won't be so fat and the components will look more like what you would expect from a desktop application.

Why Clarity?
------------

Aside from the Desktop vs Mobile issue, there are other reasons you might want to consider using Clarity.

### Material Meets Bootstrap

Now, while the end result looks a lot like Angular Material, the way you style your application feels a lot more like Bootstrap.  This is something that has always bothered me about Angular Material.  In fact, one of the main reasons we didn't use Angular Material at one of the places I've worked is specifically because it was too hard to make the site follow the design guides that were handed down to us.  Bootstrap, on the other hand, gave us an easy way to adapt the CSS to fit the requirements.  Give me components that provide functionality and provide a way that I can style them.  But don't make common components their own special component just so you can add CSS to the standard element.  Make that a common CSS thing that you can change with CSS!

### 508 Compliance

Another big motivation for me to look at Clarity is that they have stricter 508 compliance if you use the default theme.  The one that is particularly important to me is the issue of contrast to support people who are color blind like me.  That's 10% of the population that is impacted by color blindness in general which is a pretty large sample size to be pissing off with your color choices when there are tools that will let you see what it will look like to the various type of color blind people.  There compliance doesn't end there, but that is the one that impacts me, so I care more about it.

### More/Better Components

Third, several components exist that meet some requirements that the team I'm currently working on could have benefited from. While Angular Material is moving to fill in some of the gaps, the Clarity system has those components already.

### Great Documentation

While it isn't necessarily unique to Clarity, I found the documentation complete and easy to locate.  However, just like any of the other systems, you should take the time to read through most of the documentation, so you understand how it should work prior to actually trying to code anything.

### Easy to Use

Finally, I took Clarity for a spin and found it MUCH easier to use than either Bootstrap or Angular Material.  Everything worked the way I expected it to and I don't believe there was one time I had to add CSS to one of my components to make it do what I really wanted it to do.  This says a lot for a product that hasn't even been officially released yet.

### VMWare

Oh, and did I mention that it is backed by VMWare?  I always like it when an open source project is backed by a major company that doesn't appear to be going away anytime soon.  This is why I tend to stay away from some of the smaller projects, like Vue, regardless of how great they are.  Yes, I know it has a big community.  But, people need to eat.  A project with corporate sponsorship is much more likely to be run well, progress at a steady pace, and be responsive to issues that crop up.

Why You Might Want to Pass
--------------------------

For all these benefits, there are a couple of reasons why you might want to at least wait if not ignore Clarity. As I mentioned above, the product hasn't been released yet and therefore there will be breaking changes as the project makes its way to version 1.0.  This is easily mitigated by freezing your development efforts to the current release and only upgrading when you have time to address the breaking changes. You also might want to avoid using Clarity if you really need Mobile First.  I'm sure you could adapt the CSS to work with Mobile as well as Desktop.  Personally, I'd just make sure all of my business logic was outside my components so that I could develop a Mobile front-end and a Desktop front-end without losing any functionality.  This would mean one site would use Bootstrap, Angular Material, or IONIC and the other would use Clarity.

Installation - The Right Way
----------------------------

The clarity site has instructions for installing Clarity that are general enough that you could use the styles on multiple frameworks.  What follows is an adaptation of those instructions that are specific to an Angular site that uses the Angular CLI.

* `npm install --save @webcomponents/custom-elements@^1.0.0`
    * This is a polyfill that we will add to our polyfills.ts file
        
        `import '@webcomponents/custom-elements/custom-elements.min';`
        
* `npm install --save @clr/icons`
* `npm install --save @clr/ui`
* `npm install --save bootstrap@4.0.0-alpha.5`(note I didn't try using the current release version.  It may, or may not, work.)
* `npm install --save @clr/angular`
* Add the icon css and js files to the .angular-cli.json file in the "styles" and "scripts" section:

``` json    
"styles": [
    "styles.css",
    "../node_modules/@clr/icons/clr-icons.css",
    "../node_modules/@clr/ui/clr-ui.min.css"
],
"scripts": [
    "../node_modules/@clr/icons/clr-icons.min.js"
],
...
```
    
* Import the clarity module into your AppModule to make the components available

``` typescript    
import { ClarityModule } from '@clr/angular';
...

imports: [
    BrowserModule,
    ClarityModule
],
...
```
    
* `npm start` to make sure everything builds correctly.  If everything builds and you run the app, you will see that the page has bigger fonts than you may be used to.  If it looks like it has always looked, you probably don't have it configured correctly yet.

If Clarity looks like it might meet your requirements better than the alternatives, I encourage you to take [Clarity](//vmware.github.io/clarity/) for a spin.

Other Places Talking About VMWare Clarity
-----------------------------------------

* [VMware Clarity – Why should you care](//www.starwindsoftware.com/blog/vmware-clarity-why-should-you-care)
* [The Clarity Project at VMware](//devchat.tv/adv-in-angular/aia-172-clarity-project-vmware-eudes-petonnet-vincent-matt-hippely) (podcast episode)
