# INTRO


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
@NgModule({
  // ...
  imports: [
    BrowserModule,
    FormsModule,
    AppRoutingModule
  ],
  providers: [ServersService],
  bootstrap: [AppComponent]
})
export class AppModule { }

```