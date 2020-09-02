# INTRO
Observables are also like callbacks and promises - that are responsible for handling async requests. Observables are a part of the RXJS library. This library introduced Observables.


Like promises, Observables also follow the **push model**, where the producer of data is king. Producer determines **when to send data to consumer**. The Consumer does not know when data is going to come.

Here a basic example of how an observable is made:

```typescript
var observable = Rx.Observable.create((observer: any) =>{

   observer.next(‘Hi Observable’);

})

observable.subscribe((data)=>{
   console.log(data);    // output - ‘Hi Observable’
})
```

Observables may be similar to functions but they differ greatly:

```typescript
function dataProducer(){
    return ‘Hi Observable’;
    return ‘Am I understandable?’ // not a executable code.
}

var observable = Rx.Observable.create((observer: any) =>{

   observer.next(‘Hi Observable’);
   observer.next( ‘Am I understandable?’ );

})

observable.subscribe((data)=>{
   console.log(data);    
})

Output :
‘Hi Observable’
‘Am I understandable?’ 
```

From above, you can see, **both functions and observables are lazy**. We need to call (functions) or subscribe (observables) to get the results.

Subscriptions to observables are quite similar to calling a function. But where observables are different is in their ability to **.return multiple values**. called **.streams**. (a stream is a sequence of data over time).