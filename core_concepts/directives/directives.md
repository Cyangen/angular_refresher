
# Intro
Directives are the functions which will execute whenever Angular compiler finds it. Angular Directives enhance the capability of HTML elements by attaching custom behaviors to the DOM.

Angular Directive is a class with a @Directive decorator. Decorators are functions that modify JavaScript classes. The decorators are used for attaching metadata to classes as it knows the configuration of those classes and how they should work.
Components are also a directive-with-a-template. The @Component decorator is a @Directive decorator extended with template-oriented features.

## Attribute and Structural Directives
There are two types of directives: **attribute** and **structural** directives.

### Structural directives
Structural directives **change the structure of the DOM**, typically by adding, deleting, or modifying elements.
 An asterisk `(*)` precedes the directive attribute name.

```html
<!--src/app/app.component.html-->
<div *ngIf="hero" class="name">{{hero.name}}</div>
```
### Attribute directives
Attribute directives **never destroy an element** from the DOM, they **only change the appearance or behavior** of one element.
The built-in `NgStyle` Directive for example, can change several element styles at the same time.

```html
<!--app.component.html-->
<p appHighlight>Highlight me!</p>
```
```typescript
//highlight.directive.ts
import { Directive, ElementRef } from '@angular/core';

@Directive({
  selector: '[appHighlight]'
})
export class HighlightDirective {
    constructor(private elementRef: ElementRef) {
       elementRef.nativeElement.style.backgroundColor = 'yellow';
    }
}
```

Of course, you can use a better place than constructor:

```typescript
//highlight.directive.ts
import { Directive, ElementRef } from '@angular/core';

@Directive({
  selector: '[appHighlight]'
})
export class HighlightDirective implements ngOnInit {
    constructor(private elementRef: ElementRef) {}

    ngOnInit() {
      this.elementRef.nativeElement.style.backgroundColor = 'yellow';
    }
}
```

## Using Rederer to build better directives
The **Renderer2** class is an abstraction provided by Angular in the form of a service that allows to manipulate elements of your app without having to touch the DOM directly. This is the recommended approach because it then makes it easier to develop apps that can be rendered in environments that don’t have DOM access, like on the server, in a web worker or on native mobile.

```typescript
//betterHighlight.directive.ts
import { Directive, ElementRef, Renderer2 } from '@angular/core';

@Directive({
  selector: '[appBetterHighlight]'
})
export class betterHighlightDirective implements ngOnInit {
    constructor(private renderer: Renderer2, private elementRef: ElementRef) {}

    ngOnInit() {
      this.rederer.setStyle(this.elementRef.nativeElement, 'background-color', 'yellow');
    }
}
```

```html
<!--app.component.html-->
<p appBetterHighlight>Better Highlight directive!</p>
```
