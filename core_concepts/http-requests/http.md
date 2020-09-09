

## POST request
```typescript
  onCreatePost(postData: { title: string; content: string }) {
    this.http
      .post(
        'your url here',
        postData
      )
      .subscribe(responseData => {
        console.log(responseData);
      });
  }
```

## GET request
```typescript
private fetchPosts() {
  this.http.get('your url here').subscribe(posts => {
    console.log(posts);
  });
}
```
```typescript
onFetchPosts() {
  this.fetchPosts();
}
```

## RxJS Operators to Transform Response Data
```typescript
  private fetchPosts() {
  this.http.get('your url here')
  .pipe(map(responseData => {
    console.log(responseData);
    const postArray = [];
    for (const key in responseData){
      if (responseData.hasOwnProperty(key)) {
        postArray.push({...responseData[key], id: key});
      }
    }
    return postArray;
  }))
  .subscribe(posts => {console.log(posts); });
}
```

## Using Types with the HttpClient
```typescript
// post.model.ts
export interface Post {
  title: string;
  content: string;
  id?: string;
}
```
```typescript
// app.component.ts
  private fetchPosts() {
  this.http
  // define the type here
  .get<{[key: string]: Post}>('your url here')

  .pipe(map(responseData => {
    console.log(responseData);

    // define the type here as a Post array
    const postArray: Post[] = [];

    for (const key in responseData){
      if (responseData.hasOwnProperty(key)) {
        postArray.push({...responseData[key], id: key});
      }
    }
    return postArray;
  }))
  .subscribe(posts => {console.log(posts); });
}
```

## Handling errors

### Use observable error handling
```typescript
// app.component.ts
ngOnInit(){
  this.isLoading = true;
  this.postService.fetchPosts().subscribe(posts => {
    this.isLoading = false;
    this.loadedPosts = posts;
  }, error => {
    this.isLoading = false;
    console.log(error);
  });
}
```

### Using Subjects
Useful when you send request and don't subscribe to it in your component.
Especially if you have multiple places in your app that might be interested in that error.

```typescript
// post.service.ts
createAndStorePosts(title: string, content: string){
  const postData: Post = {title, content};
  this.http
    .post<{name: string}>(
      'url here',
      postData
    )
    .subscribe(responseData => {
      console.log(responseData);
    },
    error => {
      this.error.next(error.message);
    });
}
```

```typescript
// app.component.ts
error = null;
private errorSub: Subscription;

ngOnInit(){
  this.errorSub = this.postService.error.subscribe(errorMessage => {
    this.error = errorMessage;
  });
}

ngOnDestroy() {
  this.errorSub.unsubscribe();
}
```

## Setting Headers

```typescript
fetchPosts(){
  return this.http
  .get<{[key: string]: Post}>(
    '',
    {
      headers: new HttpHeaders({'custom-header': 'hello'})
    }
  )
  .pipe(map(responseData => {
    console.log(responseData);
    const postArray: Post[] = [];
    // tslint:disable-next-line: forin
    for (const key in responseData){
      if (responseData.hasOwnProperty(key)) {
        postArray.push({...responseData[key], id: key});
      }
    }
    return postArray;
  }),
  catchError(errorRes => {
    return throwError(errorRes);
  })
  );
}
```
