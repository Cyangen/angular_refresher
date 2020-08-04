
# Introduction
Databinding is an Angular feature. It allows interactions between class logic and template view (passing data).

## Binding direction
Data can bound into unidirectional or bidirectional flow. Angular technically only uses unidirectional data flow.Bidirectional flow is ultimately unidirectional. It happens in two applications of unidirectional flow, once for each direction.

Unidirectional flow defines one-way interaction. Either the component sends data to the template or the template emits an event to the component logic. Data changes within scope of the template are not percolate to the component class. Event emitting is a one-way transaction beginning from the template’s elements.

Bidirectional constitutes both directions. This means changes to the data in the class logic or template HTML persist across each other. The scope of the changes are the component’s view. The view comprises the component’s class and template together.

## Property Binding
Property binding flows a value in one direction, from a component's property into a target element property.

```javascript
// my.component.ts
@Component({
  templateUrl: './my.component.html'
})

export class MyComponent {
  value:type = /* some value of type */;
}
```

```html
<!-- my.component.html -->
<any-element [property]=“value”>innerHTML</any-element>
```

Do not confuse object properties with a DOM element’s attributes:
`attr` is a single property of the underlying DOM object. It gets declared at the DOM’s instantiation with attribute values matching the element’s definition. It maintains the same value after that.
Properties each have their own key-value field in a DOM object node. These properties are mutable post-instantiation.
Angular binds component data to properties, not attributes!

Referring back to the example, the `[ … ]` in the element’s property assignment have special meaning. The brackets show that `property` is bound to `“value”` on the right of the assignment.
`value` also has special meaning within context of the brackets. `value` by itself is a string literal. Angular reads it and matches its value against component class members. Angular will substitute the value of the matching member attribute. This of course refers to the same component class that hosts the template HTML.

Data values can also show up in a component’s `innerHTML`:

```html
<div>The value of the component class member ‘value’ is {{value}}.</div>
```

## Event Binding
This works similarly to property binding.


```javascript
// my.component.ts
@Component({
  templateUrl: './my.component.html'
})

export class MyComponent {
  handler(event):void {
      // function does stuff
  }
}
```

```html
// my.component.html
<any-element (event)=“handler($event)”>innerHTML</any-element>
```

The `(event)` pertains to any valid event type. For example, one of the most common event types is `click`. It emits when you click your mouse. Regardless of the type, event is bound to “handler” in the example. Event handlers are usually member functions of the component class.

The `( … )` are special to Angular. Parenthesis tell Angular an `event` is bounded to the right assignment of handler. The event itself originates from the host element.

When the event does `emit`, it passes the Event object in the form of $event. The `handler` maps to the identically named handler function of the component class. The unidirectional exchange from the event-bound element to the component class is complete.

Emitting events from the handler, while possible, do not impact the template element. The binding is unidirectional after all.

# Bidirectional Binding

```javascript
// my.component.ts
@Component({
  templateUrl: ‘./my.component.html’
})

export class MyComponent {
  inputValue:string = "";
}
```

```html
<!-- my.component.html -->
<input [(ngModel)]=“inputValue”>
```
