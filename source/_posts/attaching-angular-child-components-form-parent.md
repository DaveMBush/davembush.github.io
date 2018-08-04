---
title: Attaching an Angular Child Component's Form to a Parent
tags:
  - angular 2
  - child component
  - reactive forms
url: 4533.html
id: 4533
comments: false
categories:
  - Angular 2
date: 2018-01-02 06:30:57
---

This past week I implemented a pattern I've been pondering for almost a year now.  I like to create rather modular and granular code such that if my data structures are nested, the components that represent them on the screen should be nested as well. The question becomes, how does one create a reactive form in a child component and attach that form to the parent form in a way that:

1.  Leaves the definition of the child form entirely in the child
2.  Leaves the processing of the data in the parent where the parent form is the "Smart Component" and the child is a "Dumb Component"

Most solutions I was able to find attack this problem assuming the child component will be part of an array of controls.  And I suppose, if you wanted to, you could implement that pattern using an array with one element.  But, that just felt like a hack.  If you are interested in that solution, this is the wrong article. <figure>![](/uploads/2018/01/2018-01-02.jpg "Attaching an Angular Child Component's Form to a Parent") Photo by [loomingy1](//visualhunt.com/author/e29ed9) on [Visual hunt](//visualhunt.com/re/b9f011) / [ CC BY](//creativecommons.org/licenses/by/2.0/)</figure>

<!-- more --> 

Avoiding the Problem
--------------------

Up until recently, I've been able to avoid this problem entirely by creating separate forms and listening to each individually.  I could have done the same thing here.  But, every time I use this solution, I feel like there must be a better way.  Besides, when you do this correctly, you only need to have one place where you are listening for changes to the form and one place where you send changes into the form.  The work around requires multiples of each.

Basics of Nested Forms
----------------------

Another way of avoiding the problem is to create the form as one monolithic component.  A very simple implementation would be to have a form that looked something like this:

``` html
<form [formGroup]="form">
  <input type="text" formControlName="mainInput">
  <div formGroupName="childGroup">
    <input type="text" formControlName="childInput"
  </div>
</form>
```

And the reactive definition in the typescript that looked like this:

``` typescript
export class AppComponent {
  form: FormGroup;
  constructor(formBuilder: FormBuilder) {
    this.form = formBuilder.group({
      mainInput: '',
      childGroup: formBuilder.group({
        childInput: ''
      })
    });
  }
}
```

And then we would grab the data from the valueChanges observable or patch the data into the form using 

``` typescript
childGroup.childInput
```

Creating the Child Component
----------------------------

But what if you we want everything in the childGroup to be a control?  That control would look something like this: 

``` html
<div [formGroup]="form"> 
  <input type="text" formControlName="childInput"> 
</div>
```

and then our TypeScript code looks like a normal formGroup:

``` typescript
this.form = formBuilder.group({
  childInput: ''
});
```

Embedding the Child in the Parent
---------------------------------

Now that we have a separate component for our child form, we can use normal directives to add it into our main form.

``` html
<form [formGroup]="form">
  <input type="text" formControlName="mainInput">
  <app-child>
  </app-child>
</form>
```

And our TypeScript code now only needs a reference to mainInput.

``` typescript
    constructor(formBuilder: FormBuilder) {
      this.form = formBuilder.group({
        mainInput: ''
      });
    }
```

Connecting the Child to the Parent
----------------------------------

The problem is, whenever the childInput element changes, the parent form's valueChanges observer won't get notified because the parent form no longer knows about the child form.  And this is where things interesting.

Hooking the child form to the parent form is actually pretty straight forward.  The trick is knowing when in the component life-cycle to run the code.

The first thing to know is that we aren't going to be able to hookup the child in the parent until after the child component has been created.  This happens after ngOnInit() so we need to find another lifecycle hook to wire everything up in.  It just so happens that ngAfterViewInit() is the perfect place for this. 

Second, it might be tempting to hookup everything in the child component during it's ngOnInit() method.  But this would too tightly couple the child component to the parent.  Something we would like to avoid. 

So, the next thing we need to do is that we need to use @ViewChild() to allow the parent to get a hold of the child component, and ultimately the formGroup member variable it will initialize for us.

``` typescript
@ViewChild(ChildComponent) childComponent: ChildComponent;
```

And now in `ngAfterViewInit()` we can add the child's `FormGroup` as an additional "control" and set the parent `FormGroup` to the parent control's `FormGroup`.

``` typescript
ngAfterViewInit() {
  this.form.addControl('childForm', this.childComponent.form);
  this.childComponent.form.setParent(this.form);
}
```

And finally, you'll want to subscribe to `valueChanges` and your NgRX `Store` in `ngAfterViewInit()` after this wire-up code.
