# INTRO
Observables are also like callbacks and promises - that are responsible for handling async requests. Observables are a part of the RXJS library. This library introduced Observables.


## How an observable is made
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

Subscriptions to observables are quite similar to calling a function. But where observables are different is in their ability to **.return multiple values**. called **streams**. (a stream is a sequence of data over time).


Observables not only able to return a value **synchronously**, but also **asynchronously**.

```typescript
var observable = Rx.Observable.create((observer: any) =>{
   observer.next(‘Hi Observable’);
    setTimeout(()=>{
        observer.next(‘Yes, somehow understandable!’)
    }, 1000)   

   observer.next( ‘Am I understandable?’ );
})

output :

‘Hi Observable’
‘Am I understandable?’ 
Yes, somehow understandable!’.
```

In short, you can say **observables are simply a function that are able to give multiple values over time, either synchronously or asynchronously**.


## Observable Phases
There are four stages through which observables pass:

* Creation
* Subscription.
* Execution
* Destruction.

### Creation
Creation of an observable is done using a **create** function.

```typescript
var observable = Rx.Observable.create((observer: any) =>{})
```

### Subscription
To make an observable work, we have to **subscribe it**. This can be done using the **subscribe** method.

```typescript
observable.subscribe((data)=>{
   console.log(data);    
})
```

### Execution
Execution of observables is what is inside of the create block.

```typescript
var observable = Rx.Observable.create((observer: any) =>{

   observer.next(‘Hi Observable’);        
    setTimeout(()=>{
        observer.next(‘Yes, somehow understandable!’)
    }, 1000)   

   observer.next( ‘Am I understandable?’ );

})
```
The above code inside the create function is observable execution. The three types of values that an observable can deliver to the subscriber are:

```typescript
observer.next(‘hii’);//this can be multiple (more than one)

observer.error(‘error occurs’) // this call whenever any error occus.

Observer.complete(‘completion of delivery of all values’) // this tells the subscriptions to observable is completed. No delivery is going to take place after this statement.
```

### Desctruction
Last phase that comes into the market is destruction. After an error or a complete notification, the observable is automatically unsubscribed. But there are cases where we have to manually **unsubscribe** it. To manually do this task, just use:

```typescript
var subscription = observable.subscribe(x => console.log(x)); // Later: subscription.unsubscribe();
```
