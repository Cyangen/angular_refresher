# INTRO
 Interceptors transform the outgoing request before passing it to the next interceptor in the chain, by calling next.handle(transformedReq). An interceptor may transform the response event stream as well, by applying additional RxJS operators on the stream returned by next.handle().
 Another nice thing about interceptors is that they can process the request and response together.

### Immutable req and res
Although interceptors are capable of mutating requests and responses, the `HttpRequest` and `HttpResponse` instance properties are **read-only**, rendering them largely immutable.

### Execution order
Angular applies interceptors in the order that you provide them. If you provide interceptors **A**, then **B**, then **C**, requests will flow in **A->B->C** and responses will flow out **C->B->A**.


## Basic Interceptor
You have to create a new file named `nameYouWant-interceptor.service.ts`, and in this file:

```typescript
import { HttpInterceptor, HttpRequest, HttpHandler } from '@angular/common/http';

export class LoggingInterceptorService implements HttpInterceptor {
  intercept(req: HttpRequest<any>, next: HttpHandler) {
    return next.handle(req);
  }
}
```

The `HttpInterceptor` interface force you to use the `intercept()` method (it will be called everytime our application makes an HTTP request) it accept two objects as arguments: `HttpRequest` and `HttpHandler`.

When the `intercept()` method is called Angular **passes a reference** to the `httpRequest` object. With this request, you can inspect it and modify it as necessary. Once our logic is complete, we call `next.handle` and return the updated request onto the application.

Once our Interceptor is created, we need to **register** it as a multi-provider since there can be multiple interceptors running within an application.
You must register the provider to the `app.module `for it to properly apply to all application HTTP requests:

```typescript
// imports...

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule,
    HttpClientModule
  ],
  // here
  providers: [
    {
      provide: HTTP_INTERCEPTORS,
      useClass: AuthInterceptorService,
      multi: true
    },
    {
      provide: HTTP_INTERCEPTORS,
      useClass: LoggingInterceptorService,
      multi: true
    }
  ],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

## HTTP Header Interceptor
Often we need to return an API key to an authenticated API endpoint via a request Header. 
Using Interceptors, you can simplify our application code to handle this automatically:

```typescript
import { HttpInterceptor, HttpRequest, HttpHandler } from '@angular/common/http';

export class AuthInterceptorService implements HttpInterceptor {
  intercept(req: HttpRequest<any>, next: HttpHandler) {
    const API_KEY = '123456';
    return next.handle(httpRequest.clone({ setHeaders: { API_KEY } }));
  }
}
```
On the httpRequest object, we can call the clone method to modify the request object and return a new copy.
In this example we are attaching the API_KEY value as a header to every HTTP request `httpRequest.clone({ setHeaders: { API_KEY } })`.

## Interacting with the response
You can interact with the response of course, for example by log it:

```typescript
import { HttpInterceptor, HttpRequest, HttpHandler } from '@angular/common/http';

export class AuthInterceptorService implements HttpInterceptor {
  intercept(req: HttpRequest<any>, next: HttpHandler) {
    const API_KEY = '123456';
    return next.handle(httpRequest).pipe(
      tap(event => {
        console.log(event);
        if(event.type === HttpEventType.response) {
          console.log(event.body);
        }
      })
    );
  }
}
```

For interacting, you have to use the `pipe()` method on `next.handle(httpRequest)` and use all `RxJS` operators you want,
like `map()` for example. 