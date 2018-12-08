---
title: Angular(2+) Model Driven Forms Are Superior
tags:
  - angular
  - model driven
  - reactive forms
url: 4290.html
id: 4290
categories:
  - Angular
date: 2017-04-18 06:30:00
---

If you are programming in Angular and haven’t tried Model Driven Forms yet, I’m assuming that is because you’ve not taken the time to try to learn it. In this article, I am going to try to convince you that the Model Driven Form based approach is superior to Template Driven Forms and that the only people that are still using Template Driven Forms are people who either have not been enlightened or lazy.

<figure>![](/uploads/2017/04/image-4.png "Angular(2+) Model Driven Forms Are Superior")<figcaption>Photo credit: [DarlingJack](//www.flickr.com/photos/aceofknaves/33346081006/) via [Visualhunt.com](//visualhunt.com/re/f8175d) / [ CC BY](//creativecommons.org/licenses/by/2.0/)</figcaption></figure>

<!-- more -->

## What are Template Driven Forms

For those who aren’t already familiar with the terms, let’s define them first.  A template driven form is an Angular form that has most of the form logic in the template code.  The elements that give away the fact that we are working with a template driven form are that we are using ngModel in our form fields, all of our form fields have a name attribute, and our form has ngForm declared as assigned to the form variable.

``` html
<form #form="ngForm">
    <input name="name" type="text" [(ngModel)]="nameField">
</form>
```

In our code, each form field is handled individually.  While we might bind them all to a structure of some sort in our TypeScript code, the result is we continue to think of the data as parts rather than wholes.

## What are Model Driven Forms

Model Driven Forms, on the other hand, put a minimal amount of information in the template. It isn’t that we eliminate the template completely, we just put more of the responsibility into the TypeScript code.

Typical template code for Model Driven Forms looks like this

``` html
<form [formGroup]="form" >
    <input formControlName="name">
</form>
```

Notice how much less code is needed.

But, you may ask, how do I get the code in and out of the field? How do I validate the field?

Oh, but you see, that is exactly why I love Model Driven forms. That’s all in the TypeScript code.

But other than the fact that there is less code in the template and we can handle everything about the form in our TypeScript, the main difference between Template Driven Forms and Model Driven Forms is that Model Driven Forms let us treat the form as a whole rather than individual parts. This solves problems that used to be rather tricky using Template Driven Forms.

## Flexible Validation

Just so we have a reference, this is what our TypeScript code would look like to wire up our TypeScript code to our template.

``` typescript
this.form = formBuilder.group({
  name: ['',Validators.required],
  sex: ['',Validators.required],
  dob: ['',
    Validators.compose([ Validators.required, View.isDate])]
});
```

`this.form` references a public member in the form class. It is the same name that we named our form group in the template file.

Within the form group, we have a property for each field in our form group. In the sample above, we only created one input field named ‘name’. This form group references two other fields, ‘sex’ and ‘dob’. I added these in so I could show you some other features.

You’ll see that we’ve defined some validations. Simple required validations until we get to ‘dob’ where we want to make sure we give them a real date. For this, we use two new features. We use the `compose` feature to combine several validations into one. We also created a custom validation called `isDate.`

Custom validations are static methods that take the control they are associated with as a parameter. Each control has a value property that you can use to retrieve the current value of the control.

The problem that this solves is that now we can write any validation we need without having to make it a directive. We could also cross validate between this control and another control on the form by climbing up to the parent Form Group and back down to a sibling Form Control. It really is quite flexible.

## Easy Change Detection

If you are working with Template Driven Forms, you know that the way you know a value in a control has changed is because the property it is associated with gets a new value. Setting up a simple setter lets you know the field has changed. Or if you prefer you can use the split syntax of

`[ngModel]='field' (ngModelChange)='changeHander($event)'`

In Model Driven forms we can tell when any field has changed by subscribing to the valueChanges property.

``` typescript
this.form.valueChanges.subscribe((value) => {
  this.contact = deepAssign({}, this.contact, value);
});
```

The value that gets passed in has information about the field or fields that changed. So, rather than getting every field every time, you are only getting the information that actually changed. So, in your subscription, you can detect what changed and deal with that field individually if you need to. In the code above, I’m just dispatching the new state to my reducer. By doing this, my store has the current state so that when I’m ready to put it in a database, I don’t have to go around my form and gather up all the information. I already have it all.

## Centralized Form Handling

And that’s what I mean by “Centralized Form Handling.” All my form validation stuff is centralized. All the code I need to get the data out of my form is centralized. And, as you’ll see soon, all the code I need to get the data into my form is centralized.

## Completing the Picture

The only bit we have left out is, how do we get the data into the form?

That’s pretty easy.

``` typescript
this.form.patchValue({
  name: contact.name,
  sex:contact.sex,
  dob:contact.dob.toLocaleDateString()
});
```

Where contract is an object that has the new data I want to put into the corresponding fields.

## Testing

One thing we haven’t talked about is my favorite subject of testing. Because all of our logic resides in our TypeScript file, testing our screen logic becomes almost trivial. In fact, if you’ve done this correctly, you shouldn’t need to write anything more than a set of Unit Tests to make sure your screen works as expected. If you marry this with NgRX/Store, you will be even better off because you’ll never have to deal with a real database while testing your screen. And you won’t have to do a lot of mocking to achieve this.

## Conclusion

So, maybe this hasn’t convinced you. So, here is a challenge. Try it! While I have had people reject this model when I explain it to them, those who have actually tried it have seen that it really is a superior model. Which just goes to show that my dad was right, “It is amazing how much I don’t understand, when it doesn’t fit my plan.”  

## Further Your Education:

[Reactive Forms in Angular](//blog.thoughtram.io/angular/2016/06/22/model-driven-forms-in-angular-2.html) [Using Angular’s Model Driven Forms](//scotch.io/tutorials/using-angular-2s-model-driven-forms-with-formgroup-and-formcontrol) [Angular Reactive Form Fundamentals](//toddmotto.com/angular-2-forms-reactive) [Reactive Forms](//angular.io/docs/ts/latest/guide/reactive-forms.html) (From the Angular Site)
