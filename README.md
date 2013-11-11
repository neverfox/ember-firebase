ember-firebase is a stable, [thoroughly-tested](https://github.com/mjijackson/ember-firebase/tree/master/test) set of [Firebase](https://www.firebase.com/index.html) bindings for [Ember.js](http://emberjs.com/).

### Usage

ember-firebase includes support both for objects and arrays.

```js
var myRef = new Firebase('https://ember-firebase.firebaseio.com');

var object = Firebase.Object.create({ query: myRef });

object.set('myKey', 'myValue');
object.get('myKey'); // "myValue"

object.set('myKey', { my: 'value' });
object.get('myKey'); // Firebase.Object

object.set('myKey', [ 1, 2, 3 ].toFirebaseValue());
object.get('myKey'); // Firebase.Array

object.set('myKey', [ 1, 2, 3 ]);
object.get('myKey'); // Firebase.Object

var array = Firebase.Array.create({ query: myRef });

array.addObject('myValue');
array.get('length'); // 1
array.objectAt(0); // "myValue"

array.clear();
array.get('length'); // 0

array.pushObjectWithPriority('second', 2);
array.pushObjectWithPriority('first', 1);
array.objectAt(0); // "first"
array.objectAt(1); // "second"
```

It also includes several utility methods that make it convenient to get/set values in Firebase.

```js
Firebase.set(myRef, 'myValue').then(function () {
  // the value has been synced to Firebase
});

Firebase.get(myRef).then(function (value) {
  // value is the value at myRef
});

Firebase.push(myRef, 'myValue').then(function (childRef) {
  // childRef is a reference to the newly created child location
});
```

### Saving plain arrays

Since there is no native support for arrays in Firebase, they must be converted to a slightly different format before they are sent to the Firebase servers. ember-firebase provides the `Array.prototype.toFirebaseValue` array method for easily converting to values that will automatically be interpreted as `Firebase.Array`s when they are read.

```js
Firebase.set(myRef, [ 1, 2, 3 ].toFirebaseValue())
  .then(Firebase.get)
  .then(function (value) {
    // value is a Firebase.Array
  });
```

Note: You can also use `Firebase.getArrayValue` if you don't want to extend the `Array` prototype.

### License

Please feel free to use this library under the terms of the [MIT license](http://opensource.org/licenses/MIT).
