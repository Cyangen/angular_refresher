As we know, promises are for handling async requests and observables can also do the same. But where do they differ?

### Observables are lazy whereas promises are not
This is pretty self-explanatory: observables are lazy, that is we have to subscribe observables to get the results. In the case of promises, they execute immediately.

### Observables handle multiple values unlike promises
Promises can only provide a single value whereas observables can give you multiple values.

### Observables are cancelable
You can cancel observables by unsubscribing it using the unsubscribe method whereas promises don’t have such a feature.

### Observables provide many operators
There are many operators like `map`, `forEach`, `filter` etc. Observables provide these whereas promises does not have any operators in their bucket.