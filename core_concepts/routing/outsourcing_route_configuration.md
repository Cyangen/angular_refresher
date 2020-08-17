# INTRO
In Angular, the best practice is to load and configure the router in a separate, top-level module that is dedicated to routing and imported by the root AppModule.

By convention, the module class name is `AppRoutingModule` and it belongs in the `app-routing.module.ts` in the `src/app` folder.

Use the CLI command:

```cli
ng generate module app-routing --flat --module=app
```

- --flat puts the file in src/app instead of its own folder.
- --module=app tells the CLI to register it in the imports array of the AppModule.


```typescript
// app-routing.module.ts
import { NgModule } from '@angular/core';
import { Routes, RouterModule } from '@angular/router';

// component imports here

const appRoutes: Routes = [
  { path: '', component: HomeComponent},
  { path: 'users', component: UsersComponent, children: [
    { path: ':id/:name', component: UserComponent},
  ]},
  { path: 'servers', component: ServersComponent, children: [
    { path: ':id', component: ServerComponent},
    { path: ':id/edit', component: EditServerComponent}
  ]}
];

@NgModule({
  imports: [RouterModule.forRoot(appRoutes)],
  exports: [RouterModule] // make modules ready to export in others modules

})
export class AppRoutingModule {

}
```

```typescript
// app.module.ts
import { NgModule } from '@angular/core';
import { AppRoutingModule } from './app-routing.module';

@NgModule({
  declarations: [
    // ...
  ],
  imports: [
    AppRoutingModule
  ],
  providers: [ServersService],
  bootstrap: [AppComponent]
})
export class AppModule { }

```