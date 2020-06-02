# Key points - JS - Part 3

\* Estimated Time: less than 1 hour

## 1) What are promises and how they are useful?

1- We use promises for handling asynchronous interactions in a sequential
manner.

2- The promise represents the future value. It has an internal state (`pending`, `fulfilled` and `rejected`) and works like a state machine.

3- A promise object has `then` method, where you can specify what to do when the
promise is `fulfilled` or `rejected`.

_Note:_

1- `async/await` which makes the code appear even more linear.

2- `RxJS observables` can be viewed as the recyclable promises.

## 2) Fix the bug using `ES5` only (Important)

```js
var arr = [10, 32, 65, 2];

for (var i = 0; i < arr.length; i++) {
  setTimeout(function () {
    console.log("The index of this number is: " + i);
  }, 3000);
}
```

You may notice that function prints same value. The solution is by binding
`setTimeout` to the current value in the loop. This can be done by creating
[IIFE](https://www.youtube.com/watch?v=3cbiZV4H22c) (Immediately invoked
function expression):

```js
(function (j) {})(j);
```

The

```js
var arr = [10, 32, 65, 2];

for (var i = 0; i < arr.length; i++) {
  setTimeout(
    (function (j) {
      return function () {
        console.log("The index of this number is: " + j);
      };
    })(i),
    3000
  );
}
```

### What is the ES6 solution?

> using `let` instead of `var`

ECMAScript 6 (ES6) introduces new `let` and `const` keywords that are scoped
differently than `var`-based variables.

In a loop with a let-based index, each iteration through the loop
will have a new variable `i` with loop scope.

### Using `forEach`

The idea is that each invocation of the callback function used with the .forEach
loop will be its own closure.

```js
var arr = [10, 32, 65, 2];

arr.forEach(function (i) {
  setTimeout(function () {
    console.log("The index of this number is: " + i);
  }, 3000);
});
```

## 3) How does object inherit from another object without invoking a constructor?

> Object.create()

```js
const person = {
  isHuman: false,
  printIntroduction: function () {
    console.log(`My name is ${this.name}. Am I human? ${this.isHuman}`);
  },
};

const me = Object.create(person);

me.name = "Matthew"; // "name" is a property set on "me", but not on "person"
me.isHuman = true; // inherited properties can be overwritten
```

## 4) How we can prevent modification of object?

### 1- Prevent extensions

prevents new properties from ever being added to an object.

```js
const object1 = {};

Object.preventExtensions(object1);

try {
  Object.defineProperty(object1, "property1", {
    value: 42,
  });
} catch (e) {
  console.log(e);
  // Expected output: TypeError: Cannot define property property1, object is not extensible
}
```

> `preventExtensions` prevents you form adding new property BUT you can modify
> existing property or deleting it.

```js
const object1 = {
  foo: "bar",
};

Object.preventExtensions(object1);

Object.defineProperty(object1, "foo", {
  value: "baz",
});

// object1 = {
//   foo: "baz",
// };
```

### 2- Seal

Prevents new properties from being added/deleted.

```js
const object1 = {
  foo: "bar",
};

Object.seal(object1);

object1.foo = "bza";
console.log(object1.foo);
// expected output: baz

delete object1.property1; // cannot delete when sealed
console.log(object1.foo);
// expected output: baz
```

> `Seal` prevents you form adding/deleting new property BUT you can modify
> existing one.

### 3- Freeze

The strongest choice. Freezes an object. A frozen object can no longer be
changed.

```js
const obj = {
  prop: 42,
};

Object.freeze(obj);

// won't effect the object
obj.prop = 33;
// Throws an error in strict mode

// won't effect the object
delete obj.prop;

// won't effect the object
obj.prop2 = "foo";

console.log(obj);
// expected output: 42
```

> `freeze` prevents you form adding/deleting/modifying object

## 5) Write a log function which will add prefix (your message) to every message you log using `console.log`

```js
function appLog() {
  var args = Array.prototype.slice.call(arguments);
  args.unshift("your app name");
  console.log.apply(console, args);
}

appLog("Some error message");
//output of above console: 'your app name Some error message'
```

## 6) What is non-enumerable property in JavaScript and how you can create one?

> properties that don't show up when you iterate through object using for...in loop or using Object.keys()

```js
var person = {
  name: "John",
};
person.salary = "10000$";
person["country"] = "USA";

console.log(Object.keys(person)); // ['name', 'salary', 'country']

// Create non-enumerable property
Object.defineProperty(person, "phoneNo", {
  value: "8888888888",
  enumerable: false,
});

console.log(Object.keys(person)); // ['name', 'salary', 'country']
```

In the example above phoneNo property didn't show up because we made it
non-enumerable by setting `enumerable:false`

_Note:_ `Object.defineProperty()` also lets you create read-only properties. So,
we cannot modify phoneNo value of a person object.

## Disclaimer

This is cloned from famous
[123-Essential-JavaScript-Interview-Questions](https://github.com/ganqqwerty/123-Essential-JavaScript-Interview-Questions)
re-written with different distribution and focused more in tricky questions.
With extra resources [JavaScript closure
inside loops – simple practical
example](https://stackoverflow.com/questions/750486/javascript-closure-inside-loops-simple-practical-example),
[Object.seal](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/seal),
[Object.preventExtensions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/preventExtensions)
and
[Object.freeze](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze).
