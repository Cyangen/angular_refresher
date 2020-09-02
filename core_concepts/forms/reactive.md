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