# INTRO
for the template-driver approach you need to import the `FormsModule` module in your `app.module.ts`.

When Angular detects the `<form>` element in your html code, it creates a javascript representation of the form as a JS Object.

Important thing, Angular do not detect `inputs` inside your form, leaving you free to choose which inputs you want to add.
So, you have to register controls manually and in TD approach this is super simple by using `ngModel` directive and HTML attribute `name`:

```html
<input type="text" id="username" class="form-control" ngModel name="username">
```

## ngSubmit directive
The `ngSubmit` directive is used by Angular to make a form submittable.
You can place on this `<form>` element as a whole:

```html
<form (ngSubmit)="onSubmit()">
  ...
</form>
```

`ngSubmit` actually only gives you one event you can listen to,so let's wrap it in parentheses.
This event made available by the `ngSubmit` directive will be fired whenever this form is submitted, so whenever this default behavior is triggered.
At this point, you can call a function from your .ts file when a user click on a submit button:

```typescript
  // app.component.ts
  OnSubmit() {
    console.log('form submitted')
  }
```

## Accessing the JS object
To access to the JS object created automatically by Angular from `<form>` html element, you have to use a local reference equals to `ngForm` and then, pass that to your method:

```html
<form (ngSubmit)="onSubmit(f)" #f="ngForm">
  ...
</form>
```

then:

```typescript
  // app.component.ts
  OnSubmit(form: ngForm) {
    console.log(form)
  }
```

## Accessing the Form with @ViewChild
This is an alternative approach using @ViewChild.

@ViewChild allowed you to access a local reference, an element controlled or which holds a local reference in our TypeScript code.

You do just have such a local reference here:

```html
<form (ngSubmit)="onSubmit()" #f="ngForm">
  ...
</form>
```
and whilst that might not point to an element ref but to the ngForm object, it still is a local reference.

```typescript
export class AppComponent {
  @ViewChild('f') signUpForm:ngForm;

  onSubmit(form: NgForm) {
    console.log(this.signUpForm);
  }
}
```
In your .ts file you can access to the element which has the local reference f on it using @ViewChild decorator.

## Input validation
In the template-driven approach, you can only add this in the template:

```html
<input type="text" id="username" class="form-control" ngModel name="username" required >
```

`required` is a default HTML attribute,you can add to an input, however Angular will detect it,
and it acts as a selector for a built-in directive shipping with Angular and it will automatically configure
your form, to take this into account, to make sure that now this will be treated as `invalid` if it is empty.

### Dynamyc css classes by form state
Moreover Angular traks the **state of each control** of the form (you can check that by having a look to JS object) 
and consequently, dynamically add some CSS classes, giving you information about the state of the individual control (if it is dirty or not,if you did change the initial value, if it touched or not, if you clicked into it or not or if it is valid or not).

With that informations, you can style these inputs conditionally.

For example you can disable the submit button if form is invalid and style it:

```html
<button class="btn btn-primary" type="submit" [disabled]="!f.valid">Submit</button>
```
You can disable the submit button dynamically by the form state, in this case if the form is invalid: `[disabled]="!f.valid"`
```css
input.ng-invalid.ng-touched{
  border: 1px solid red;
}
```
`.ng-invalid` `.ng-touched` are css classes added by Angular when input validation fail. In this case, inputs will get a red  border if touched by user and if its will be invalid (according with your validation config).

### Outputting Validation Error Messages

```html
  <input type="email" id="email" class="form-control" ngModel name="email" required email #email="ngModel">
  <span *ngIf="!email.valid && email.touched" class="help-block">enter a valid email address</span>
```
A quick and easy way of getting access to the control created by Angular automatically is adding by adding a local reference equals to `ngModel` to this input here.
So just like the form directive automatically added by Angular when it detects a form element, the
`ngModel` directive here exposes some additional information about the control it creates for us on the overarching form by accessing ngModel.
So with this, we could simply check or say that we want to attach this span here

### Which Validators do ship with Angular? 
Check out the Validators class: <https://angular.io/api/forms/Validators> - these are all built-in validators, though that are the methods which actually get executed (and which you later can add when using the reactive approach).

For the template-driven approach, you need the directives. You can find out their names, by searching for "validator" in the official docs: <https://angular.io/api?type=directive> - everything marked with "D" is a directive and can be added to your template.

Additionally, you might also want to enable HTML5 validation (by default, Angular disables it). You can do so by adding the ngNativeValidate  to a control in your template.

## Set Default Values with ngModel Property Binding

```html
<select id="secret" class="form-control" [ngModel]="defaultQuestion" name="secret">
  <option value="pet">Your first Pet?</option>
  <option value="teacher">Your first teacher?</option>
</select>
```
```typescript
export class AppComponent {
  defaultQuestion = 'pet';

  onSubmit(form: NgForm) {
    console.log(this.signUpForm);
  }
}
```

## Using ngModel with Two-Way-Binding
Sometimes you not only want to pre-populate a default the user entered but you also want to instantly react to any changes.

for example:

```html
<div class="form-group">
  <textarea class="form-control" name="questionAnswer" rows="3" [(ngModel)]="answer"></textarea>
</div>
<p>Your reply: {{ answer }}</p>
```
```typescript
export class AppComponent {
  answer = '';

  onSubmit(form: NgForm) {
    console.log(this.signUpForm);
  }
}
```

## Patching Form Values
Sometimes you want to programmatically patch a input value in your form avoiding overwriting others controls.
You can use `patchValue()` method to obtain this:

```typescript
export class AppComponent {
  @ViewChild('f') signUpForm:ngForm;

  suggestUserName() {
    const suggestedName = 'Superuser';
    this.signUpForm.form.patchValue({
      username: suggestedName
    });
  }
}
```

```html
<button class="btn btn-default" type="button" (click)="suggestUserName()">Suggest an Username</button>
```

## Reset form values and proprietes
You can easy reset all form inputs values, propreties and validation status by using `reset()` method:

```typescript
export class AppComponent {
  @ViewChild('f') signUpForm:ngForm;

  suggestUserName() {
    const suggestedName = 'Superuser';
    this.signUpForm.reset();
  }
}
```