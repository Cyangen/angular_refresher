# INTRO
Reactive forms provide a model-driven approach to handling form inputs whose values change over time. This guide shows you how to create and update a basic form control, progress to using multiple controls in a group, validate form values, and create dynamic forms where you can add or remove controls at run time.

## Setup
make sure to import `ReactiveFormsModule` in your `app.module.ts`:

```typescript
@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    ReactiveFormsModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
```

and `formGroup` in your `component.ts` file:

```typescript
import { FormGroup } from '@angular/forms';
```

## Form creation
```typescript
// app.component.ts
export class AppComponent implements OnInit {
  genders = ['male', 'female'];

  signUpForm: FormGroup;

  ngOnInit(){
    this.signUpForm = new FormGroup({
      'username': new FormControl('default username'),
      'email': new FormControl(null),
      'gender': new FormControl('male')
    });
  }
}
```

Controls are basically just **key-value pairs** in the object we pass in `FormGroup()`.
`new FormControl()` takes some parameters: 

* the initial **value** of the control
* a **single** or an **array** of validators
* **asynchronous** validators


## Syncing HTML and Form
First of all we need a **directive** that overrides angular's default behavior in recognizing an html form: `formGroup` and bind to it our property `signUpForm` that holds our **form**:

```html
<form [formGroup]="signUpForm"></form>
```

Second step we need to tell Angular which controls should be connected to which inputs in the templace code, using the `formControlName` directive:

```html
<input type="text" id="email"class="form-control" formControlName="email">
```

we have to pass, as a string, the property name we declared inside the object passed in `FormGroup`.

The final result will be:

```html
<form [formGroup]="signUpForm">
  <div class="form-group">
    <label for="username">Username</label>
    <input
      type="text"
      id="username"
      class="form-control"
      formControlName="username">
  </div>
  <div class="form-group">
    <label for="email">email</label>
    <input
      type="text"
      id="email"
      class="form-control"
      formControlName="email">
  </div>
  <div class="radio" *ngFor="let gender of genders">
    <label>
      <input
        formControlName="gender"
        type="radio"
        [value]="gender">{{ gender }}
    </label>
  </div>
  <button class="btn btn-primary" type="submit">Submit</button>
</form>
```

```typescript
// app.component.ts
export class AppComponent implements OnInit {
  genders = ['male', 'female'];

  signUpForm: FormGroup;

  ngOnInit(){
    this.signUpForm = new FormGroup({
      'username': new FormControl('default username'),
      'email': new FormControl(null),
      'gender': new FormControl('male')
    });
  }
}
```

## Submitting the form
You can submit the form simply using the `ngSubmit` directive:

```html
<form [formGroup]="signUpForm" (ngSubmit)="onSubmit()"></form>
```

```typescript
// app.component.ts
export class AppComponent implements OnInit {
  genders = ['male', 'female'];

  signUpForm: FormGroup;

  ngOnInit(){
    this.signUpForm = new FormGroup({
      'username': new FormControl('default username'),
      'email': new FormControl(null),
      'gender': new FormControl('male')
    });
  }

  onSubmit(){
    console.log(this.signUpForm);
  }
}
```

## Validation
```typescript
  ngOnInit(){
    this.signUpForm = new FormGroup({
      'username': new FormControl(null, Validators.required),
      'email': new FormControl(null, [Validators.required, Validators.email]),
      'gender': new FormControl('male')
    });
  }
```

Do non use `()` on `Validators` methods, because you don't want to call it.
it is a static method made available by validators here,instead you only want to pass the reference to this method.
Angular will execute the method whenever it detects that the input of this FormControl changed, so it just needs to have a reference on what it should execute at this point of time.

## Getting access to controls
In the reactive approach you can use the `signUpForm.get()` method.
It allows you to get access to your controls easily:

```html
<div class="form-group">
  <label for="username">Username</label>
  <input
    type="text"
    id="username"
    class="form-control"
    formControlName="username">
  <span class="help-block" *ngIf="signUpForm.get('username').valid && signUpForm.get('username').touched">please enter a valid username</span>
</div>
```

## Dynamic adding form controls

```typescript
export class AppComponent implements OnInit {

  signUpForm: FormGroup;

  ngOnInit(){
    this.signUpForm = new FormGroup({
      'username': new FormControl(null, Validators.required),
      'email': new FormControl(null, [Validators.required, Validators.email]),
      'gender': new FormControl('male'),
      'hobbies': new FormArray([])
    });
  }

  getControls() {
    return (<FormArray>this.signUpForm.get('hobbies')).controls;
  }

  onAddHobby(){
    const control = new FormControl(null, Validators.required);
    (<FormArray>this.signUpForm.get('hobbies')).push(control)
  }
}
```
First of all, create a new `FormArray` control in your `signUpForm`, this control will be your array of dynamic controls (hobby) added by user.

`onAddHobby()` will be fired when the user clicks on your "add hobby" button (see below in HTML code), it will **create** a `new FormControl` (your hobby) and **push** it in your hobbies controls array.

**IMPORTANT:** make sure to tell TypeScript that this is of type FormArray to not get an error, by using `(<FormArray>)` when you get your **hobbies controls array**. Like this:


```typescript
(<FormArray>this.signUpForm.get('hobbies')).push(control)
```

now, everything enclosed in these outer parentheses is treated as `FormArray`,so now you can push a new control on this array, if you would have not casted this, you would get an error.

`getControls()` is a method that let you get all your hobby controls for looping through `*ngFfor` in your html template.

Normally you would have done in this way:

```html
<!-- !!!!NOT WORKING!!!! -->
<div class="form-group" *ngFor="let hobby of signUpForm.get('hobbies').controls; let i = index">
  <input type="text" class="form-control" [formControlName]="i">
</div>
```

But this code will produce an error.
`getControls()` is an adjustment due to the way TS works and Angular parses your templates (it doesn't understand TS there).

Finally you have to update your html template:

```html
<div formArrayName="hobbies">
  <h4>hobbies</h4>
  <button class="btn btn-default" type="button" (click)="onAddHobby()">add hobby</button>
  <div class="form-group" *ngFor="let hobby of getControls(); let i = index">
    <input type="text" class="form-control" [formControlName]="i">
  </div>
</div>
```

`formArrayName` used to link your template to your ts code.
`*ngFor="let hobby of getControls(); let i = index"` used to extract array element index to be used as name in `[formControlName]` and link correctly your control.


## Custom validators

A validator in the end is just a function which gets executed by Angular automatically when it checks the validity of the FormControl and it checks that validity whenever you change that control.

```typescript
forbittenUserNames = ['anna','rock'];

ngOnInit(){
  this.signUpForm = new FormGroup({
    'username': new FormControl(null, [Validators.required, this.forbittenNames.bind(this)]),
    'email': new FormControl(null, [Validators.required, Validators.email]),
    'gender': new FormControl('male'),
    'hobbies': new FormArray([])
  });
}

forbittenNames(control: FormControl): {[s:string]: boolean} {
  if(this.forbittenUserNames.indexOf(control.value) !== -1){
    return {'nameIsForbitten':true}
  }
  return null;
}
```
Now a validator to work correctly needs to receive an argument which is the control it should check,
so this will be of type FormControl, a validator also needs to return something for Angular to be
able to handle the return value correctly.

`{[s:string]: boolean}` is what type of return we expect, in this case an object with a **key string** and a **boolean value**


## Using Error Codes





## Custom Async Validator
typically, you might need to reach out to a web server to check this.
That however is an asynchronous operation because the response is not coming back instantly, instead it just takes a couple of seconds.

```typescript
ngOnInit(){
  this.signUpForm = new FormGroup({
    'email': new FormControl(null, [Validators.required, Validators.email], this.forbittenEmails),
  });
}

forbittenEmails(control: FormControl): Promise<any> | Observable<any> {
  const promise = new Promise<any>((resolve, reject) => {
    setTimeout(() => {
      if(control.value === "test@gmail.com"){
        resolve({'emailIsForbitten': true});
      } else {
        resolve(null);
      }
    },1500)
  });
  return promise;
}
```

## Tracking Status and Values changes of your form
You can track constantly status and salues **changes** of your form thanks to a couple of **observables**


```typescript
ngOnInit(){
  this.signUpForm = new FormGroup({
    'username': new FormControl(null, [Validators.required, this.forbittenNames.bind(this)]),
    'email': new FormControl(null, [Validators.required, Validators.email], this.forbittenEmails),
    'gender': new FormControl('male'),
    'hobbies': new FormArray([])
  });

  this.signUpForm.valueChanges.subscribe(
    (value) => console.log(value)
  );

  this.signUpForm.statusChanges.subscribe(
    (value) => console.log(value)
  );
}
```