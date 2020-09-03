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
If you want to display something based on your form state, in the reactive approach you can use the `signUpForm.get()` method.
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