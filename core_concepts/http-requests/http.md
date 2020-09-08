

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