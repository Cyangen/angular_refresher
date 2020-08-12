## simple redirect

This is the way to make a simple redirect:

```typescript
// app.module.ts
const appRoutes: Routes = [
  { path: 'something', redirectTo: '/other-route'},
];
```

By default, Angular matches paths by prefix. That means, that the following route will match both `/recipes`  and just `/` 

```typescript
{ path: '', redirectTo: '/somewhere-else' } 
```

Actually, Angular will give you an error here, because that's a common gotcha: This route will now ALWAYS redirect you! Why?
Since the default matching strategy is "prefix" , Angular checks if the path you entered in the URL does start with the path specified in the route. Of course every path starts with ''  (Important: That's no whitespace, it's simply "nothing").
To fix this behavior, you need to change the matching strategy to "full" :

```typescript
{ path: '', redirectTo: '/somewhere-else', pathMatch: 'full' } 
```

Now, you only get redirected, if the full path is `''`.

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
Make sure to put `**` route as **last route** in your `Routes` list


