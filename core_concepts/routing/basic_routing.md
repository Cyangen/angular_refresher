# Intro



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
        <!-- routerLink directive used to navigate preventing reloading page behaviuor -->
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
