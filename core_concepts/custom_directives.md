# Intro
Quick example of how create a custom Directives in angular.


## Attribute Directive



### src/app/highlight.directive.ts
```typescript
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

### src/app/app.module.ts
Don't forget to add your directive in app.module.ts declarations:

```typescript
@NgModule({
  declarations: [
    AppComponent,
    HighlightDirective
  ],
  imports: [
    BrowserModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

### src/app/app.component.html
```html
<!--src/app/app.component.html-->
<p appHighlight>Highlight me!</p>
```
