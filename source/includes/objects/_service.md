# Service

## extend

Create a custom service object.

```js
var CreateUser = Utensils.Service.extend();
```

## argumentName

```js
var CreateUser = Utensils.Service.extend({

  argumentName: 'user'

});

var createUser = new CreateUser({firstName: 'John', lastName: 'Smith'});
createUser.user; // {firstName: 'John', lastName: 'Smith'}
```

The name of the the argument passed into the constructor. This value is used as the key for the value when it is stored on the instance of the object. Accepts an array value for when multiple arguments are passed in.

## error

```js
var CreateUser = Utensils.Service.extend({
  
  error: function(err){ }

});
```

If the procedure errors at any point, whether a deferred was rejected or an error was thrown, an optional error method can be defined and will be executed on failure of the procedure chain.

## run

```js
new CreateUser().run(); // executes the procedure chain
```

Executes the procedure chain.

If a value is passed into the `run` method, it will be exposed as the first argument of the first operation in the procedure chain.

```js
var CreateUser = Utensils.Service.extend({

  procedure: [
    'save',
    ...
  ],

  save: function( num ) {
    num // 2
  }

});

new CreateUser().run(2);
```

`run()` always returns a promise, so you can chain subsequent operations off of it using `then` and `fail` and any other callback types available on the [Q library](https://github.com/kriskowal/q#propagation).

```js
new CreateUser().run(2)
  .then(function() {
    // successful procedure
  })
  .fail(function() {
    // something broke
  })
```

## procedure

An array of strings corresponding to methods on the service object.  The order in which they are specified in this array determines the order of operations at the time the service is executed with `run()`.

```js
var CreateUser = Utensils.Service.extend({
  
  procedure: [
    'save',
    'sendWelcomeEmail'
  ],

  save: function(){ },

  sendWelcomeEmail: function(){ }

});
```

## Custom Methods

The foundation of a service object is for the developer to write custom methods that perform the operations desired for that action.  Utensils provides some useful functionality to help make this quick and easy.

Any method that is specified in the `procedure` Array will be bound to the object and called in the order in which it is specified.  Any values returned from a custom method are passed to the subsequent method in the procedure.

If any of the operations in the procedure are asynchronous, an optional `resolve` and `reject` argument may be defined on the custom method to wrap the method in a promise and have these two functions defined.

### resolve

The `resolve` method exposed to an asynchronous operation is used when the operation has performed successfully, and the procedure should move to the next operation.  This method optionally accepts a single argument that can be passed to the subsequent operation in the procedure chain.

```js
var CreateUser = Utensils.Service.extend({
  
  sendEmail: function( res, resolve, reject ) {
    resolve({foo: 'bar'});
  },

  analytics: function( res ) {
    res // {foo: 'bar'}
  }

});
```

### reject

The `reject` method exposed to an asynchronous operation is used when the operation has failed.  Executing `resolve` will break the chain and immediately fail the entire procedure.  This method optionally accepts a single argument, for use in identifying the error that occurred.

```js
var CreateUser = Utensils.Service.extend({
  
  sendEmail: function( res, resolve, reject ) {
    reject(new Error('Something went wrong'));
  },

  analytics: function( res ) {
    // never executed
  }

});
```