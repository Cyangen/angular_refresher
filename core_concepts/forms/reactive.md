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