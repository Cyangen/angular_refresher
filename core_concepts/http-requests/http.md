

## POST request

```typescript
  onCreatePost(postData: { title: string; content: string }) {
    // Send Http request
    this.http
      .post(
        'your endpoint here',
        postData
      )
      .subscribe(responseData => {
        console.log(responseData);
      });
  }
  ```