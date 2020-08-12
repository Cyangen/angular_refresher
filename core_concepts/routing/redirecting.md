## simple redirect

This is the way to make a simple redirect:

```typescript
// app.module.ts
{ path: 'something' redirectTo: '/not-found'}
```

## wildcard routes and 404 page

Use a wildcard route to handle when users attempt to navigate to a part of your application that does not exist.
The Angular router selects this route any time the requested URL doesn't match any router paths.
This example combine `redirectTo` with `'**'`:

```typescript
// app.module.ts
{ path: 'not-found', component: PageNotFoundComponent},
{ path: '**' redirectTo: '/not-found'}
```

When a user try to navigate to unknow route, he will be redirected to a 404 page.


