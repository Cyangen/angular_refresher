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

In `user.compoent.ts`, `.params` is an observable.