# Key points - JS - Part 2

\* Estimated Time: 1 hour

## 1) functions

### Which function is defined in `run time` and which is in `parse time`?

```js
var foo = function () {
  // Some code
};
```

```js
function bar() {
  // Some code
}
```

- `foo` is defined at `run-time` and is called a function expression.
- `bar` is defined at `parse time` and is called a function statement.

Run-Time function declaration:

```js
foo(); // Call foo function here, It will give an error
var foo = function () {};
```

Parse-Time function declaration:

```js
bar(); // Call bar function here, It will not give an Error
function bar() {}
```

### What is the output of the following:

```js
bar();
(function abc() {
  console.log("John");
})();
function bar() {
  console.log("Doe");
}
```

The output:

```bash
Doe
John
```

### Hoisting: execution contexts

> expected: The usual

```js
function catName(name) {
  console.log("My cat's name is " + name);
}

catName("Tiger");

// output: "My cat's name is Tiger"
```

> expected: because of parse time declaration

```js
catName("Tiger");

function catName(name) {
  console.log("My cat's name is " + name);
}

// output: "My cat's name is Tiger"
```

> Only declarations are hoisted

```js
console.log(num); // undefined, as only declaration was hoisted, no initialization has happened at this stage
var num; // declaration
num = 6;
```

Inside functions:

```js
var salary = "1000"; // declaration => hoisted

(function () {
  console.log("Original salary was " + salary); // undefined

  salary = "5000";

  console.log("My New Salary " + salary); // 5000
})();
```

## 2) Difference between `Function`, `Method` and `Constructor`

- Methods in JavaScript are object properties

```js
var obj = {
  helloWorld: function () {
    return "hello world, " + this.name;
  },
  name: "John Carter",
};
obj.helloWorld(); // // "hello world John Carter"
```

_Notice_ how `helloWorld` refers to `this` properties of obj

> The unexpected

```js
var obj2 = {
  helloWorld: obj.helloWorld,
  name: "John Doe",
};
obj2.helloWorld(); // "hello world John Doe
```

Why? Because this refers now to `obj2`. So, `name` is `John Doe`.

- `this` in functions is `undefined`

```js
var obj1 = {
  prop1: "buddy",
};

var myFunc = function () {
  console.log("Hi there", this);
};

myFunc(); // will output "Hi there undefined" or "Hi there Window"

obj1.myMethod = myFunc;
obj1.myMethod(); // will print "Hi there" following with obj1.
```

- `constructors` creates a brand new object and passes it as the value of this,
  and implicitly returns the new object as its result.

```js
function Employee(name, age) {
  this.name = name;
  this.age = age;
}

var emp1 = new Employee("John Doe", 28);
emp1.name; // "John Doe"
emp1.age; // 28
```

### What would be the output of the following:

```js
function User(name) {
  this.name = name || "JsGeeks";
}

var person = (new User("xyz")["location"] = "USA");
console.log(person);
```

- first it creates object `person {name:xyz}`
- then, it assigns new property `person.location = "USA"`

So, the output is `USA`.

```js
// person = {
//   name: "xyz",
//   location: "USA",
// };
```

## 2) What are Service Workers and when can you use them?

> It’s a technology that allows your web application to use cached resources
> first, and provide default experience offline, before getting more data from
> the network later.

## 3) Describe Singleton Pattern In JavaScript

1- It's a design pattern.

2- It provides a way to wrap the code into a logical unit that can be accessed
through a single variable.

3- One of the best ways to prevent accidentally overwriting variable is to
namespace your code within a singleton object.

> It is an object that is used to create namespace and group together a related
> set of methods and attributes (encapsulation) and if we allow to initiate then
> it can be initiated only once.

```js
/* Singleton */

var mySingleton = (function () {
  // Instance stores a reference to the Singleton
  var instance;

  function init() {
    // Singleton
    // Private methods and variables
    function privateMethod() {
      console.log("I am private");
    }
    var privateVariable = "Im also private";
    var privateRandomNumber = Math.random();
    return {
      // Public methods and variables
      publicMethod: function () {
        console.log("The public can see me!");
      },
      publicProperty: "I am also public",
      getRandomNumber: function () {
        return privateRandomNumber;
      },
    };
  }
  return {
    // Get the Singleton instance if one exists
    // or create one if it doesn't
    getInstance: function () {
      if (!instance) {
        instance = init();
      }
      return instance;
    },
  };
})();

var singleA = mySingleton.getInstance();
var singleB = mySingleton.getInstance();
singleA.publicMethod();
console.log(singleA.publicProperty);
console.log(singleA.getRandomNumber() === singleB.getRandomNumber());
```

## 4) What are the ways of creating objects?

### 1- Function based

```js
function Employee(fName, lName, age, salary) {
  this.firstName = fName;
  this.lastName = lName;
}

var employee1 = new Employee("John", "Doe");
var employee2 = new Employee("Jane", "Doe");
```

### 2- Function constructor with prototype

```js
function Person() {}
Person.prototype.name = "John Doe";
var object = new Person();
```

### 3- Object Literal

```js
var employee = {
  fName: "John",
  lName: "Doe",
  getName: function () {
    return this.name;
  },
};
```

### 4- From `Object` using `new` keyword (Object constructor)

```js
var employee = new Object(); // Created employee object using new keywords and Object()
employee.name = "John Doe";
employee.getName = function () {
  return this.name;
};
```

### 5- Using Object.create

`Object.create(obj)` will create a new object and set the `obj` as its
`prototype`. You can use `Object.create(null`) when you don’t want your object to
inherit the properties of Object.

### 6- ES6 Class syntax

```js
class Person {
  constructor(name) {
    this.name = name;
  }
}

var object = new Person("John Doe");
```

## 5) Write a function called `deepClone` which takes an object and creates a copy of it

```js
function deepClone(object) {
  var newObject = {};

  var keys = Object.keys(object);

  function isObject(obj) {
    return typeof obj === "object" && object[key] !== null;
  }

  for (let i = 0; i < keys.length; i += 1) {
    var key = keys[i];
    var currentObj = object[key];

    if (isObject(currentObj)) {
      newObject[key] = deepClone(currentObj);
    } else {
      newObject[key] = currentObj;
    }
  }

  return newObject;
}
```

it works recursively with:

```js
var personalDetail = {
  name: "John",
  address: {
    location: "xyz",
    zip: "123456",
    phoneNumber: {
      homePhone: 012345678,
      workPhone: 012345678,
    },
  },
};
```

## 6) What's best way to detect `undefined`?

> We can use `typeof` operator to check `undefined`

```js
if (typeof someProperty === "undefined") {
  console.log("something is undefined here");
}
```

## Disclaimer

This is cloned from famous
[123-Essential-JavaScript-Interview-Questions](https://github.com/ganqqwerty/123-Essential-JavaScript-Interview-Questions)
re-written with different distribution and focused more in tricky questions.
With extra resources [Singleton Design Pattern - Beau teaches
JavaScript](https://www.youtube.com/watch?v=bgU7FeiWKzc)
and [Hoisting](https://developer.mozilla.org/en-US/docs/Glossary/Hoisting).
