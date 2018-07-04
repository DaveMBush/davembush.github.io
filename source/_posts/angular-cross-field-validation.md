---
title: Angular Cross Field Validation
tags:
  - angular
  - formgroup
  - validation
url: 4528.html
id: 4528
comments: false
categories:
  - Angular 2
date: 2017-12-19 06:30:36
---

This past week I had my first need to do use cross field validation in Angular.  While the general mechanics are pretty trivial, my particular implementation ran into some issues that you might be interested in. <figure>![](/uploads/2017/12/2017-12-19.jpg "Angular Cross Field Validation") Photo by [MSVG](//visualhunt.com/author/525a6d) on [VisualHunt](//visualhunt.com/re/2a53de) / [ CC BY](//creativecommons.org/licenses/by/2.0/)</figure>

<!-- more --> 

The Basics
----------

As I said, the mechanics of implementing cross field validation in Angular is rather trivial.  It all hinges on the concept of a FieldGroup which is a key concept of [Reactive Forms](/tags/reactive-forms/). What we need to do to implement cross field validation is to attach a validation function to the form instead of the field. I'm going to use the AppComponent to host the FormBuilder for simplicity:

``` typescript
export class AppComponent {
  form:FormGroup;

  constructor(formBuilder: FormBuilder) {
    this.form=formBuilder.group({
      // form field definitions here
    }, {
      validator: AppComponent.formGroupValidationFunction
    });
  }
}
```

You'll notice that `formBuilder.group()` takes a second parameter which takes a `validator` function or function array.  This parameter can also the `asyncValidator` key or the `state` key. The function we are pointing to takes the `FormGroup` as a parameter.  So, within the function, we can access the controls that are part of the `FormGroup`.  Once we have the fields, we can access the values of the fields and perform whatever comparisons we need, which is pretty trivial.  Then, if there is an error, we call setError() on the control(s) that are impacted.

``` typescript
static formGroupValidationFunction(formGroup: FormGroup):void {
  constfield1=formGroup.controls['field1'];
  constfield2=formGroup.controls['field2'];
  // compare field1 to field2
  if(error) {
    field1.setError({formGroupValidationFunction:true});
    field2.setError({formGroupValidationFunction:true});
  }
}
```

Dealing with Field Validations
------------------------------

One of the problems I ran into was that my fields also had individual validations on them.  Specifically, the two fields were numbers that I was validating to make sure they were positive and only displayed two decimal places.  By the time I entered the validation for the FormGroup, that validation had already run.  I also wanted to clear any pre-existing errors from my form validation. It turns out that the way Angular determines if there is an error is if the forms errors object exist.  If it is null, it is assumed there aren't any errors. So, to clear the pre-existing errors, the safest thing to do is to first delete the error I was adding from each field, and then check to see if there are any other errors in the errors object.  If there aren't any errors, we then call `setError(null)` to clear out the error object.

``` typescript
if(field1.errors&&field1.errors.formGroupValidationFunction) {
  deletefield1.errors.formGroupValidationFunction;
  if(Object.keys(field1.errors).length===0) {
    field1.setErrors(null);
  }
}
```

When adding the `FormGroup` error, we only call `setError()` if the errors object is null.

``` typescript
if(!field1.errors) {
  field1.setError({ formGroupValidationFunction:true });
}
```

By doing this, we ensure that the field validation errors aren't overwritten by the `FormGroup` validations.

Alternatives
------------

The code I've shown works well enough if you only have a one-off validation.  But in my case, I needed to use the validation between multiple sets of fields.  To do this, I created a function that returns another function. The outer function takes two parameters.  Strings that are keys into the controls of the form group.  So, now instead of:

``` typescript
constructor(formBuilder:FormBuilder) {
  this.form=formBuilder.group({
    // form stuff here
  }, {
    validator:formGroupValidationFunctionHere
  });
}
```

I have:

``` typescript
constructor(formBuilder:FormBuilder) {
  this.form=formBuilder.group({
    // form stuff here
  }, {
    validator:formGroupValidationFunction('field1', 'field2')
  });
}
```

And my validation function looks something like this:

``` typescript
static formGroupValidationFunction(f1: string, f2: string): Function {
  return (formGroup: FormGroup): void => {
    constfield1=formGroup.controls[f1];
    constfield2=formGroup.controls[f2];
    // compare field1 to field2
    if(error) {
      field1.setError({formGroupValidationFunction: true});
      field2.setError({formGroupValidationFunction: true});
    }
  }
}
```

You'll notice that my `setError()` uses the name of the function as the error key.  I just do this for clarity.  You CAN name it whatever you want.  I name it the same to be consistent with how the Angular validations work. Finally, I like to put my custom validations in a separate Static Class rather than including them in the component code.  I've only placed them in the component code here for illustrative purposes.
