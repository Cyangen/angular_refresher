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

Moreover  Angular dynamically add some CSS classes, giving you information about the state of the individual control (if it is dirty or not,if you did change the initial value, if it touched or not, if you clicked into it or not or if it is valid or not).
With that informations, you can style these inputs conditionally.

### Which Validators do ship with Angular? 

Check out the Validators class: <https://angular.io/api/forms/Validators> - these are all built-in validators, though that are the methods which actually get executed (and which you later can add when using the reactive approach).

For the template-driven approach, you need the directives. You can find out their names, by searching for "validator" in the official docs: <https://angular.io/api?type=directive> - everything marked with "D" is a directive and can be added to your template.

Additionally, you might also want to enable HTML5 validation (by default, Angular disables it). You can do so by adding the ngNativeValidate  to a control in your template.