

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