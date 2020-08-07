# Intro
In a **single-page app**, you change what the user sees by showing or hiding portions of the display that correspond to particular components, rather than going out to the server to get a new page. As users perform application tasks, they need to move between the different views that you have defined. To implement this kind of navigation within the single page of your app, you use the Angular Router.

To handle the navigation from one view to the next, you use the Angular `router`.
The router enables navigation by interpreting a browser URL as an instruction to change the view.
The router enables developers to build **Single Page Applications with multiple views** and allow navigation between these views.


```typescript
// app.module.ts
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { FormsModule } from '@angular/forms';
import { Routes, RouterModule } from '@angular/router';

import { AppComponent } from './app.component';
import { HomeComponent } from './home/home.component';
import { UsersComponent } from './users/users.component';
import { ServersComponent } from './servers/servers.component';
import { UserComponent } from './users/user/user.component';
import { EditServerComponent } from './servers/edit-server/edit-server.component';
import { ServerComponent } from './servers/server/server.component';
import { ServersService } from './servers/servers.service';

const appRoutes: Routes = [
  { path: '', component: HomeComponent},
  { path: 'users', component: UserComponent},
  { path: 'servers', component: ServersComponent}
];

@NgModule({
  declarations: [
    AppComponent,
    HomeComponent,
    UsersComponent,
    ServersComponent,
    UserComponent,
    EditServerComponent,
    ServerComponent
  ],
  imports: [
    BrowserModule,
    FormsModule,
    RouterModule.forRoot(appRoutes)// registering our array routes.
  ],
  providers: [ServersService],
  bootstrap: [AppComponent]
})
export class AppModule { }
```


```HTML
<!-- app.component.ts -->
<div class="container">
  <div class="row">
    <div class="col-xs-12 col-sm-10 col-md-8 col-sm-offset-1 col-md-offset-2">
      <ul class="nav nav-tabs">
        <!-- routerLink directive used to navigate preventing reloading page behavior -->
        <li role="presentation" class="active" routerLinkActive="active" [routerLinkActiveOptions]="{exact:true}">
          <a routerLink="/">Home</a>
        </li>
        <li role="presentation" routerLinkActive="active">
          <a routerLink="servers">Servers</a>
        </li>
        <!-- [routerLink] used to bind some non-string data -->
        <li role="presentation" routerLinkActive="active">
          <a [routerLink]="['users']">Users</a>
        </li>
      </ul>
    </div>
  </div>
  <div class="row">
    <div class="col-xs-12 col-sm-10 col-md-8 col-sm-offset-1 col-md-offset-2">
      <!-- directive that informs Angular where render the content associated to a route -->
      <router-outlet></router-outlet>
    </div>
  </div>
</div>
```

## The Router-Outlet
The Router-Outlet is a `directive` that’s available from the router library where the Router inserts the `component` that gets matched based on the current browser’s URL.
You can add multiple outlets in your Angular application which enables you to implement advanced routing scenarios.

```html
<router-outlet></router-outlet>
```
Any component that gets matched by the Router will render it as a sibling of the Router outlet.

## The routerLink directive
The Angular Router provides the `routerLink` directive to create navigation links.
RouterLink directive is used to navigate preventing reloading page behavior
