# Key points - JS - Part 1

\* Estimated Time: 1 hour

## 1) What's the difference between `undefined` and `not defined`?

- `not defined` is for undeclared variables.
- `undefined` is for declared variables without a value.

```js
var x; // declaring x
console.log(x); // output: undefined
typeof x === "undefined"; // Will return true

console.log(y); // Output: ReferenceError: y is not defined
```

## 2) For which value of x the results of the following statements are not the same?

```js
//  if( x <= 100 ) {...}
if( !(x > 100) ) {...}
```

The key point is `Nan` and `undefined`:

- `NaN <= number` and `NaN <= number` is always: `false`
- `undefined <= number` and `undefined <= number` is always: `false`

### How about `null`

> unlike `undefined` `NaN` Js treats `null` as value.

```js
null < 10; // true
null > 10; // false
null === 0; // false
```

### How about `parseInt`

- `undefined` because `parseInt(undefined) : NaN`
- `[]` because `parseInt([]) : NaN` - It parses only first element.
- `{}` because `parseInt({}) : NaN` - applied to empty or full object.

The following returns `Nan`

```js
parseInt([]);
parseInt(["foo", "bar"]);
parseInt({});
parseInt({ foo: "bar" });
parseInt(undefined);
parseInt(null);
```

The following returns `1`

```js
parseInt([1, 2, 3]); // 1
parseInt(["1", "2", "3"]); // 1

parseInt(["2", "3"]); // 2
parseInt([2, 3]); // 2
```

### Parsing `boolean`

- `true` is `1`
- `false` is `0`

```js
var bar = true;
console.log(bar + 0); // 1
console.log(bar + true); // 2
console.log(bar + false); // 1
```

_Note:_ Adding `boolean` to string will parse it to string

```js
var bar = true;
console.log(bar + "Foo"); // trueFoo
```

## 3) What is the drawback of declaring methods directly in objects?

> Performance.

```js
var Employee = function (firstName, lastName) {
  this.firstName = firstName;
  this.lastName = lastName;

  // We can create a method like this:
  this.fullName = function () {
    return `${this.firstName} ${this.lastName}`;
  };
};

// we can also create method in Employee's prototype:
Employee.prototype.fullName2 = function () {
  return `${this.firstName} ${this.lastName}`;
};

//creating objects
var emp1 = new Employee("John", "Doe");
var emp2 = new Employee("Jane", "Doe");
```

### What's the difference between `fullName` and `fullName2`

For each new object created they will have their own copy of `fullName` method
while `fullName2` will only be added once to an object Employee.prototype

## 4) What is “closure”?

> a function defined inside another function.

_Note:_ The closure has access to the variable in three scopes:

- Variable declared in his own scope.
- Variable declared in parent function scope.
- Variable declared in the global namespace.

```js
var globalVar = "abc";

// Parent self invoking function
(function outerFunction(outerArg) {
  // begin of scope outerFunction
  // Variable declared in outerFunction function scope
  var outerFuncVar = "x";
  // Closure self-invoking function
  (function innerFunction(innerArg) {
    // begin of scope innerFunction
    // variable declared in innerFunction function scope
    var innerFuncVar = "y";
    // end of scope innerFunction
  })(5); // Pass 5 as parameter
  // end of scope outerFunction
})(7); // Pass 7 as parameter
```

### The expected output is

```js
outerArg = 7;
outerFuncVar = x;
innerArg = 5;
innerFuncVar = y;
globalVar = abc;
```

## 5) Write a mul function which will work properly when invoked with following syntax:

```js
console.log(mul(2)(3)(4)); // output : 24
console.log(mul(4)(3)(4)); // output : 48
```

### The expected answer

```js
function mull(x) {
  return function (y) {
    return function (z) {
      return x * y * z;
    };
  };
}
```

## 6) How to empty an array?

### Method 1: array = []

> It doesn't affect referenced array

```js
var arrayList = ["a", "b"]; // Created array
var referencedArr = arrayList; // Referenced arrayList by another variable

arrayList = []; // Empty original array
console.log(referencedArr); // Output ['a', 'b']
```

### Method 2: array.length = 0

> It affects referenced array

```js
arrayList.length = 0; // Empty original array
console.log(referencedArr); // Output []
```

### Method 3: Using splice(0, array.length)

> It affects referenced array

```js
arrayList.splice(0, arrayList.length); // Empty original array
console.log(referencedArr); // Output []
```

### Method 4: Using pop

> It affects referenced array

```js
// Empty original array
while (arrayList.length) {
  arrayList.pop();
}
console.log(referencedArr); // Output []
```

## How to check if a given variable is an array?

In modern browser:

> Array.isArray(arrayList)

Or:

> Object.prototype.toString.call(arrayList) === '[object Array]'

## 9) The `delete` operator

> it doesn't affect local variables

```js
var output = (function (x) {
  delete x;
  return x;
})(0);

console.log(output); // 0
```

> it doesn't affect global variables

```js
var x = 1;

var output = (function () {
  delete x;
  return x;
})(0);

console.log(output); // 0
```

> It's used to delete a property from an object

```js
var x = { foo: 1 };
var output = (function () {
  delete x.foo;
  return x.foo;
})();

console.log(output); // undefined
```

> It doesn't delete prototype property

```js
var Employee = {
  foo: "bar",
};

var emp1 = Object.create(Employee);

delete emp1.foo;

console.log(emp1.company); // "bar"
```

_Explanation:_ emp1 object doesn't have foo as its own property instead it has
it chained: `emp1.__proto__.foo`

This works:

```ts
delete emp1.__proto__.foo;
```

> it doesn't affect array length

```js
var fruits = ["apple", "orange"];
console.log(fruits.length); // 2

delete fruits[1]; // ["apple", empty]
console.log(fruits.length); // 2
```

_Note:_

Some browsers like `Chrome` uses `undefined x 1` to fill the deleted
element. recently they've started to use `empty` for displaying it.

However `fruits[1] === undefined` in any browser.

## 10) typeof

### What will be the output of the following code:

<!-- prettier-ignore -->
```js
var z = 1, y = z = typeof y;
console.log(y); // undefined
```

Another curious case:

```js
var foo = function bar() {
  return 12;
};
typeof bar(); // Reference Error
```

While:

```js
var bar = function () {
  return 12;
};
typeof bar(); // "number"
```

## difference between `typeof` and `instanceof`

- The `typeof` operator checks if a value belongs to one of the seven basic
  types: `number`, `string`, `boolean`, `object`, `function`, `undefined` or `Symbol`.

- The `instanceof` operator works on the level of prototype.

```js
var dog = new Animal();
dog instanceof Animal;
```

## Disclaimer

This is cloned from famous
[123-Essential-JavaScript-Interview-Questions](https://github.com/ganqqwerty/123-Essential-JavaScript-Interview-Questions)
re-written with different distribution and focused more in tricky questions. All
credits go to the authors and contributes of the original repo.
