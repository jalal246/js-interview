# Coding Questions - JS - Part 1

\* Estimated Time: 1 hour 20 minutes

> What would be the output of following code?

## Passing by reference & by value

## Q1

```js
var strA = "hi there";
var strB = strA;

strB = "bye there!";

console.log(strA);
```

> The output is: `hi there`

## Q2

```js
var objA = { prop1: 42 };
var objB = objA;

objB.prop1 = 90;

console.log(objA);
```

> The output is: `{prop1: 90}`

## Q3

```js
var objA = { prop1: 42 };
var objB = objA;

objB = {};

console.log(objA);
```

> The output is: `{prop1: 42}`

_why?_ when we reassign `objB` to an empty object, we simply change where `objB` variable references to. This doesn't affect where `objA` variable references to.

## Q4

```js
var arrA = [0, 1, 2, 3, 4, 5];
var arrB = arrA;

arrB[0] = 42;

console.log(arrA);
```

> The output is: `[42,1,2,3,4,5]`

## Q5

```js
var arrA = [0, 1, 2, 3, 4, 5];
var arrB = arrA.slice();

arrB[0] = 42;

console.log(arrA);
```

> The output is: `[0,1,2,3,4,5]`

## Q6

```js
var arrA = [
  { prop1: "value of array A!!" },
  { someProp: "also value of array A!" },
  3,
  4,
  5,
];
var arrB = arrA.slice();

arrB[0].prop1 = 42;
arrB[3] = 20;

console.log(arrA);
```

> The output is: `[{ prop1: 42 }, { someProp: "also value of array A!" }, 3, 4, 5]`

## Hoisting

## Q7

```js
console.log(employeeId);
var employeeId = "19000";
```

> The output is: `undefined`

## Q8

```js
var employeeId = "1234abe";

(function () {
  console.log(employeeId);
  var employeeId = "122345";
})();
```

> The output is: `undefined`

## Q9

```js
var employeeId = "1234abe";

(function () {
  console.log(employeeId);
  var employeeId = "122345";
  (function () {
    var employeeId = "abc1234";
  })();
})();
```

> The output is: `undefined`

## Q10

```js
(function () {
  console.log(typeof displayFunc);
  var displayFunc = function () {
    console.log("Hi I am inside displayFunc");
  };
})();
```

> The output is: `undefined`

## Q11

```js
var employeeId = "abc123";

function foo() {
  employeeId = "123bcd";
  return;
}

foo();

console.log(employeeId);
```

> The output is: `123bcd`

## Q12 (So tricky!)

```js
var employeeId = "abc123";

function foo() {
  employeeId = "123bcd";
  return;

  function employeeId() {}
}

foo();

console.log(employeeId);
```

> The output is: `abc123`

## Q13

```js
var employeeId = "abc123";

function foo() {
  employeeId();
  return;

  function employeeId() {
    console.log(typeof employeeId);
  }
}

foo();
```

> The output is: `function`

## Q14

```js
function foo() {
  employeeId();
  var product = "Car";
  return;

  function employeeId() {
    console.log(product);
  }
}

foo();
```

> The output is: `undefined`

## Q15

```js
(function foo() {
  bar();

  function bar() {
    abc();
    console.log(typeof abc);
  }

  function abc() {
    console.log(typeof bar);
  }
})();
```

> The output is: `function function`

## Objects

## Q16

```js
(function () {
  "use strict";

  var person = {
    name: "John",
  };
  person.salary = "10000$";
  person["country"] = "USA";

  Object.defineProperty(person, "phoneNo", {
    value: "8888888888",
    enumerable: true,
  });

  console.log(Object.keys(person));
})();
```

> The output is: `[name, salary, country, phoneNo]`

## Q17

```js
(function () {
  "use strict";

  var person = {
    name: "John",
  };
  person.salary = "10000$";
  person["country"] = "USA";

  Object.defineProperty(person, "phoneNo", {
    value: "8888888888",
    enumerable: false,
  });

  console.log(Object.keys(person));
})();
```

> The output is: `[name, salary, country]`

## Q18

```js
(function () {
  var objA = {
    foo: "foo",
    bar: "bar",
  };
  var objB = {
    foo: "foo",
    bar: "bar",
  };
  console.log(objA == objB);
  console.log(objA === objB);
})();
```

> The output is: `false false`

## Q19

```js
(function () {
  var objA = new Object({ foo: "foo" });
  var objB = new Object({ foo: "foo" });
  console.log(objA == objB);
  console.log(objA === objB);
})();
```

> The output is: `false false`

## Q20

```js
(function () {
  var objA = Object.create({
    foo: "foo",
  });
  var objB = Object.create({
    foo: "foo",
  });
  console.log(objA == objB);
  console.log(objA === objB);
})();
```

> The output is: `false false`

## Q21

```js
(function () {
  var objA = Object.create({
    foo: "foo",
  });
  var objB = Object.create(objA);
  console.log(objA == objB);
  console.log(objA === objB);
})();
```

> The output is: `false false`

## Q22

```js
(function () {
  var objA = Object.create({
    foo: "foo",
  });
  var objB = Object.create(objA);
  console.log(objA.toString() == objB.toString());
  console.log(objA.toString() === objB.toString());
})();
```

> The output is: `true true`

## Q23

```js
(function () {
  var objA = Object.create({
    foo: "foo",
  });
  var objB = objA;
  console.log(objA == objB);
  console.log(objA === objB);
  console.log(objA.toString() == objB.toString());
  console.log(objA.toString() === objB.toString());
})();
```

> The output is: `true true true true`

## Q24

```js
(function () {
  var objA = Object.create({
    foo: "foo",
  });
  var objB = objA;
  objB.foo = "bar";
  console.log(objA.foo);
  console.log(objB.foo);
})();
```

> The output is: `bar bar`

## Q25 (Unbelievable)

```js
(function () {
  var objA = Object.create({
    foo: "foo",
  });
  var objB = objA;
  objB.foo = "bar";

  delete objA.foo;

  console.log(objA.foo);
  console.log(objB.foo);
})();
```

> The output is: `foo foo`

## Arrays

## Q25

```js
(function () {
  var array = new Array("100");
  console.log(array);
  console.log(array.length);
})();
```

> The output is: `[100] 1`

## Q26

```js
(function () {
  var array1 = [];
  var array2 = new Array(100);
  var array3 = new Array(["1", 2, "3", 4, 5.6]);
  console.log(array1);
  console.log(array2);
  console.log(array3);
  console.log(array3.length);
})();
```

> The output is: `[] (100) [empty × 100] [Array[5]] 1`

## Q27

```js
(function () {
  var array = new Array("a", "b", "c", "d", "e");
  array[10] = "f";
  delete array[10];
  console.log(array.length);
})();
```

> The output is: `11`

## Q28

```js
(function () {
  var animal = ["cow", "horse"];
  animal.push("cat");
  animal.push("dog", "rat", "goat");
  console.log(animal.length);
})();
```

> The output is: `6`

## Q29

```js
(function () {
  var animal = ["cow", "horse"];
  animal.push("cat");
  animal.unshift("dog", "rat", "goat");
  console.log(animal);
})();
```

> The output is: `[ 'dog', 'rat', 'goat', 'cow', 'horse', 'cat' ]`

## Q30

```js
(function () {
  var array = [1, 2, 3, 4, 5];
  console.log(array.indexOf(2));
  console.log([{ name: "John" }, { name: "John" }].indexOf({ name: "John" }));
  console.log([[1], [2], [3], [4]].indexOf([3]));
  console.log("abcdefgh".indexOf("e"));
})();
```

> The output is: `1 -1 -1 4`

## Q31

```js
(function () {
  var array = [1, 2, 3, 4, 5, 1, 2, 3, 4, 5, 6];
  console.log(array.indexOf(2));
  console.log(array.indexOf(2, 3));
  console.log(array.indexOf(2, 10));
})();
```

> The output is: `1 6 -1`

## Q32

```js
(function () {
  var numbers = [2, 3, 4, 8, 9, 11, 13, 12, 16];
  var even = numbers.filter(function (element, index) {
    return element % 2 === 0;
  });
  console.log(even);

  var containsDivisible3 = numbers.some(function (element, index) {
    return element % 3 === 0;
  });

  console.log(containsDivisible3);
})();
```

> The output is: `[1, 4, 8, 12, 16] true`

## Q33

```js
(function () {
  var containers = [2, 0, false, "", "12", true];
  var containers = containers.filter(Boolean);
  console.log(containers);
  var containers = containers.filter(Number);
  console.log(containers);
  var containers = containers.filter(String);
  console.log(containers);
  var containers = containers.filter(Object);
  console.log(containers);
})();
```

> The output is: `[ 2, '12', true ] [ 2, '12', true ] [ 2, '12', true ] [ 2, '12', true ]`

## Q34

```js
(function () {
  var list = ["foo", "bar", "john", "ritz"];
  console.log(list.slice(1));
  console.log(list.slice(1, 3));
  console.log(list.slice());
  console.log(list.slice(2, 2));
  console.log(list);
})();
```

> The output is:
> `["bar", "john", "ritz"] ["bar", "john"] ["foo", "bar", "john", "ritz"] [] ["foo", "bar", "john", "ritz"]`

## Q35

```js
(function () {
  var list = ["foo", "bar", "john"];
  console.log(list.splice(1));
  console.log(list.splice(1, 2));
  console.log(list);
})();
```

> The output is: `["bar", "john"] [""] ["foo"]`

## Disclaimer

This is cloned from famous
[123-Essential-JavaScript-Interview-Questions](https://github.com/ganqqwerty/123-Essential-JavaScript-Interview-Questions)
re-written with different distribution and focused more in tricky questions.
