
#Intro
Directives are the functions which will execute whenever Angular compiler finds it. Angular Directives enhance the capability of HTML elements by attaching custom behaviors to the DOM.

Angular Directive is a class with a @Directive decorator. Decorators are functions that modify JavaScript classes. The decorators are used for attaching metadata to classes as it knows the configuration of those classes and how they should work.
Components are also a directive-with-a-template. The @Component decorator is a @Directive decorator extended with template-oriented features.

## Attribute and Structural Directives
There are two types of directives: **attribute** and **structural** directives.

Structural directives **change the structure of the DOM**, typically by adding, deleting, or modifying elements.
 An asterisk `(*)` precedes the directive attribute name.

```html
<!--src/app/app.component.html-->
<div *ngIf="hero" class="name">{{hero.name}}</div>
```

Attribute directives **never destroy an element** from the DOM, they **only change the appearance or behavior** of one element.
The built-in `NgStyle` Directive for example, can change several element styles at the same time.

Here an example with **custom attribute directive**:
```html
<!--src/app/app.component.html-->
<p appHighlight>Highlight me!</p>
```
```typescript
//src/app/highlight.directive.ts
import { Directive, ElementRef } from '@angular/core';

@Directive({
  selector: '[appHighlight]'
})
export class HighlightDirective {
    constructor(private el: ElementRef) {
       el.nativeElement.style.backgroundColor = 'yellow';
    }
}
```

Of course, you can use a better place than constructor:

```typescript
//src/app/highlight.directive.ts
import { Directive, ElementRef } from '@angular/core';

@Directive({
  selector: '[appHighlight]'
})
export class HighlightDirective {
    constructor(private el: ElementRef) {}

    ngOnInit() {
      this.el.nativeElement.style.backgroundColor = 'yellow';
    }
}
```
