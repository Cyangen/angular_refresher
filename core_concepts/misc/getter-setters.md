# INTRO
Getters and setters (also known as accessors) were introduced to JavaScript when ECMAScript 5 (2009) was released.

**Getters and setters** are another way for you to provide access to the properties of an object.






##  using underscore variables
The getter/setter cannot have the name that matches the property name,
so the general pattern is to name the internal properties just like a setter/getter only with the edition of an underscore:

```typescript
class User{
  constructor(
    private _email: string
  ){}

  get email(){
    return this._email
  }
}
```

Since JS and therefore TS **lacks runtime encapsulation**, an underscore usually signifies internal/private/encapsulated property/variable.

But nothing forces you to use underscores. If you name your getter/setter differently, you can avoid adding underscores:

```typescript
class User{
  constructor(
    private email: string
  ){}

  get getEmail(){
    return this.email
  }
}
```