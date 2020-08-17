# INTRO
Web applications often consist of public and private data. Both types of data tend to have their own pages with guarded routes. These routes allow/restrict access depending on the user’s privileges. Unauthorized users may interact with a guarded route. The route should block the user if he or she attempts to access its routed content.

Angular provides a bundle of authentication guards that can attach to any route. These methods trigger automatically depending on how the user interacts with the guarded route.

- `canActivate(...)` - fires when the user attempts to access a route
- `canActivateChild(...)` - fires when the user attempts to access a route’s nested (child) routes
- `canDeactivate(...)` - fires when the user attempts to leave a route

Angular’s guard methods are available from `@angular/router`. To help them authenticate, they may optionally receive a few parameters. Such parameters do not inject via dependency injection. Under the hood, each value gets passed in as an argument to the invoked guard method.

- `ActivatedRouteSnapshot` provides access to the route parameters of the guarded route.
- `RouterStateSnapshot` exposes the URL (uniform resource locator) web address matching the route.
- `Component` references the component rendered by the route.

## CanActivate Guard

```typescript
// auth.service.ts
export class AuthService {
  loggedIn = false;

  isAuthenticated() {
    const promise = new Promise( (resolve, reject) => {
      setTimeout(() => {
        resolve(this.loggedIn);
      }, 800);
    });
    return promise;
  }

  login() {
    this.loggedIn = true;
  }

  logOut() {
    this.loggedIn = false;
  }
}
```

```typescript
// auth-guard.service.ts
import { CanActivate, ActivatedRouteSnapshot, RouterStateSnapshot, Router } from "@angular/router";
import { Observable } from "rxjs";
import { Injectable } from "@angular/core";
import { AuthService } from "./auth.service";

@Injectable()
export class AuthGuard implements CanActivate {

  constructor(private authService: AuthService, private router: Router) {}

  canActivate(route: ActivatedRouteSnapshot, state: RouterStateSnapshot): Observable<boolean> | Promise<boolean> | boolean {
    return this.authService.isAuthenticated()
    .then((authenticated: boolean) => {
      if (authenticated) {
        return true;
      } else {
        this.router.navigate(['/']);
      }
    });
  }
}
```

```typescript
// app-routing.module.ts
import { AuthGuard } from './auth-guard.service';

const appRoutes: Routes = [
  { path: 'servers', canActivate: [AuthGuard], component: ServersComponent, children: [
    { path: ':id', component: ServerComponent},
    { path: ':id/edit', component: EditServerComponent}
  ]},
];

@NgModule({
  imports: [RouterModule.forRoot(appRoutes)],
  exports: [RouterModule]
})
export class AppRoutingModule {}
```

```typescript
// app.module.ts
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { FormsModule } from '@angular/forms';
import { AppRoutingModule } from './app-routing.module';
import { AuthService } from './auth.service';
import { AuthGuard } from './auth-guard.service';

@NgModule({
  declarations: [
  ],
  imports: [
  ],
  providers: [ServersService, AuthService, AuthGuard],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

### Protecting child (nested) routes
```typescript
// auth-guard.service.ts
canActivateChild(route: ActivatedRouteSnapshot, state: RouterStateSnapshot): Observable<boolean> | Promise<boolean> | boolean {
  return this.canActivate(route, state);
}
```

```typescript
// app-routing.module.ts
const appRoutes: Routes = [
  { path: 'servers', canActivateChild: [AuthGuard], component: ServersComponent, children: [
    { path: ':id', component: ServerComponent},
    { path: ':id/edit', component: EditServerComponent}
  ]}
];
```

## CanDeactivate Guard

```typescript
// canDeactivate-guard.service.ts
import { Observable } from "rxjs";
import { CanDeactivate, ActivatedRouteSnapshot, RouterStateSnapshot } from "@angular/router";

export interface canComponentDeactivate {
  canDeactivate: () => Observable<boolean> | Promise<boolean> | boolean;
}

export class CanDeactivateGuard implements CanDeactivate<canComponentDeactivate> {
  // This is the canDeactivate method which will be called by the Angular router once we try to leave a route.
  canDeactivate( component: canComponentDeactivate,
                currentRoute: ActivatedRouteSnapshot,
                currentState: RouterStateSnapshot,
                nextState: RouterStateSnapshot): Observable<boolean> | Promise<boolean> | boolean {
                  return component.canDeactivate();
                }
}
```

```typescript
// edit-server.component.ts
import { Component, OnInit } from '@angular/core';

import { ServersService } from '../servers.service';
import { ActivatedRoute, Params, Router } from '@angular/router';
import { canComponentDeactivate } from './can-deactivate-guard.service';
import { Observable } from 'rxjs';

@Component({
  selector: 'app-edit-server',
  templateUrl: './edit-server.component.html',
  styleUrls: ['./edit-server.component.css']
})
export class EditServerComponent implements OnInit, canComponentDeactivate {
  server: {id: number, name: string, status: string};
  serverName = '';
  serverStatus = '';
  allowEdit = false;
  changesSaved = false;

  constructor(private serversService: ServersService, private route: ActivatedRoute, private router: Router) { }

  ngOnInit() {
    this.route.queryParams.subscribe((queryParams: Params) => {
      this.allowEdit = queryParams['allowEdit'] === '1' ? true : false;
    });
    this.route.fragment.subscribe();
    this.server = this.serversService.getServer(1);
    this.serverName = this.server.name;
    this.serverStatus = this.server.status;
  }

  onUpdateServer() {
    this.serversService.updateServer(this.server.id, {name: this.serverName, status: this.serverStatus});
    this.changesSaved = true;
    this.router.navigate(['../'], {relativeTo: this.route});
  }

  canDeactivate(): Observable<boolean> | Promise<boolean> | boolean {
    if (!this.allowEdit) {
      return true;
    }
    if (this.serverName !== this.server.name || this.serverStatus !== this.server.status && !this.changesSaved) {
      return confirm('leave?');
    } else {
      return true;
    }
  }
}
```