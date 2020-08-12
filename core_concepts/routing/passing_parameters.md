# Intro
Often, as a user navigates your application, you want to pass information from one component to another. For example, consider an application that displays a list of users. Each user in the list has a unique id. To edit it, you click an Edit button, which opens an EditUser component. You want that component to retrieve the id for the user so it can display the right information about your user.
You can use a route to pass this type of information to your application components.

```typescript
// app.module.ts
const appRoutes: Routes = [
  { path: 'users/:id/:name', component: UserComponent},
];
```

```typescript
//user.compoent.ts
import { Component, OnInit } from '@angular/core';
import { ActivatedRoute, Params } from '@angular/router';

@Component({
  selector: 'app-user',
  templateUrl: './user.component.html',
  styleUrls: ['./user.component.css']
})
export class UserComponent implements OnInit {
  user: {id: number, name: string};

  constructor(private route: ActivatedRoute) { }

  ngOnInit() {
    this.route.params.subscribe((params: Params) => {
      this.user.id = params['id'];
      this.user.name = params['name'];
    });
  }
}
```

```html
<!-- user.component.html -->
<p>User with ID {{ user.id }} loaded.</p>
<p>User name is {{ user.name }}</p>
<hr>
<a [routerLink] = "['/users',10,'anna']">click me</a>
```

## About Observables

In `this.route.params.subscribe`, `sparams` is an **Observable**.
Basically, observables are a feature added by some other third-party package, not by Angular but heavily
used by Angular which allow you to easily work with **asynchronous tasks**.
This is an asynchronous task because the parameters of your currently loaded route might change
at some point in the future if the user clicks this link.
You don't know when, you didn't know if and you don't know how long it will take.
So, you can't block your code and wait for this to happen here because it might never happen.
An observable is an easy way to **subscribe** to some event which might happen in the future, to then execute
some code when it happens without having to wait for it now.
That is what `params` is. It is such an observable and as the name implies, we can **observe** it and we
do so by subscribing to it.

# Query params and Fragments

### passing Query params and Fragments via RouterLink
```typescript
// app.module.ts
const appRoutes: Routes = [
  { path: 'servers/:id/edit', component: EditServerComponent}
];
```
```html
<!-- servers.component.html -->
<a
  [routerLink] = "['/servers', 5 ,'edit']"
  [queryParams] = "{allowEdit: 1}"
  fragment = "loading"
  class="list-group-item"
  >
  Click me!
</a>
```

### passing Query params and Fragments Programmatically
```html
<!--home.component.html-->
<button (click)="onLoadServers(1)">Load Servers 1 </button>
```

```typescript
  //home.component.ts
  onLoadServers(id: number) {
    // other code...
    this.router.navigate(['/servers', id, 'edit'], {queryParams: {allowEdit: '1'}, fragment: 'loading'});
  }
```

### retrieving Query params and Fragments

You can use an observables to do this:

```typescript
  import { ActivatedRoute } from '@angular/router';

  constructor(private route: ActivatedRoute) { }

  ngOnInit() {
    this.route.queryParams.subscribe();
    this.route.fragment.subscribe();
  }
```

### preserve queryParams when navigating to routes

`queryParamsHandling: 'preserve'` allow you to preserve queryParams when you navigating to other routes, for example when you use `navigate` method.

```typescript
  // server.component.ts
  onEdit() {
    this.router.navigate(['edit'], {relativeTo: this.route, queryParamsHandling: 'preserve'});
  }
  ```
