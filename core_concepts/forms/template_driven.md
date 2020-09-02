# INTRO
for the template-driver approach you need to import the `FormsModule` module in your `app.module.ts`.

When Angular detects the `<form>` element in your html code, it creates a javascript representation of the form as a JS Object.

Important thing, Angular do not detect `inputs` inside your form, leaving you free to choose which inputs you want to add.
So, you have to register controls manually and in TD approach this is super simple by using `ngModel` directive and HTML attribute `name`:

```html
<input type="text" id="username" class="form-control" ngModel name="username">
```

