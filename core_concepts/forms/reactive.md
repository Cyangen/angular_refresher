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