## simple redirect

This is the way to make a simple redirect:

```typescript
// app.module.ts
const appRoutes: Routes = [
  { path: 'something', redirectTo: '/other-route'},
];
```

## wildcard routes and 404 page

Use a wildcard route to handle when users attempt to navigate to a part of your application that does not exist.
The Angular router selects this route any time the requested URL doesn't match any router paths.
This example combine `redirectTo` with `'**'`:

```typescript
// app.module.ts
const appRoutes: Routes = [
  { path: 'not-found', component: PageNotFoundComponent},
  // ** means catch all routes that are not defined in app.module.ts, this route MUST be the last one.
  { path: '**', redirectTo: '/not-found'},

];
```

When a user try to navigate to unknow route, he will be redirected to a 404 page.
Make sure to put `**` route as **last route** in your `Routes` array


