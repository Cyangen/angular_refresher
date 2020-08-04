# Introduction
To bind data and events across different components, you must use the `@Input` and `@Output` decorators. Angular components are privately scoped. None of a component’s members are accessible from anywhere outside of its native view.

## From Parent to Child
The `@Input` decorator indicates a member’s value is sourced from the parent function.

```typescript
//parent.component.ts
@Component({
  templateUrl:'./parent.component.html'
})
export class ParentComponent {
  value:string = 'Hello World';
}
```
```html
//parent.component.html
<div>
  <app-child-component [property]="value"></app-child-component>
</div>
```


```typescript
//child.component.ts
@Component({
  selector: 'app-child-component'
  templateUrl:'./child.component.html'
})
export class ChildComponent {
  @Input() property:string;
}
```
```html
//child.component.html
<h3>{{property}}</h3>
```

## From Child to Parent
@Output shows how an event travels from child to parent. Keep in mind that @Output almost always pertains to custom event bindings.


```html
//child.component.html
<button (click)="handleClick('Hello World')">Click Me!</button>
```
```typescript
//child.component.ts
@Component({
  selector: 'app-child-component'
  templateUrl:'./child.component.html'
})
export class ChildComponent {
  @Output() customEvent = new EventEmitter();

  handleClick(message){
    this.customEvent.emit(message);
  }
}
```


```html
//parent.component.html
<div>
  <app-child-component (customEvent)="handler($event)"></app-child-component>
</div>
```
```typescript
//parent.component.ts
@Component({
  templateUrl:'./parent.component.html'
})
export class ParentComponent {
  handler(message) {
    console.log(message);
  }
}
```
