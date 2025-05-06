+++
title = 'Javascript Language Notes'
date = 2025-01-02T18:49:04+08:00
draft = false
tags = ["programming languages"]
+++

# Variables and Constants

Keywords `var` and `let` can all define variables, but differ in scope. Generally speaking, `var`'s scope is bigger than `let`, and `var` is more versatile than `let`. You should always prefer `let` to define variables whenever possible. For more differences between `var` and `let`, refer to [this post](https://www.javascripttutorial.net/difference-between-var-and-let/).

A constant holds a value that doesn’t change. To declare a constant, you use the `const` keyword. When defining a constant, you need to initialize it with a value immediately. For example:

```javascript
const workday = 5;
```

# Primary Data Types

JavaScript has the primary data types: `null`, `undefined`, `boolean`, `number`, `string`, `symbol` (available from ES2015), and `bigint` (available from ES2020). 

```javascript
let counter = 120; // counter is a number
counter = false;   // counter is now a boolean
counter = "foo";   // counter is now a string
console.log(typeof counter) // "boolean" : typeof can be used to check out the variable type
```

JavaScript uses the number type to represent both integer and floating-point numbers. Note that JavaScript automatically converts a floating-point number into an integer if the number appears to be a whole number. To get the range of the number type, you use `Number.MIN_VALUE` and `Number.MAX_VALUE`. Also, you can use `Infinity` and `-Infinity` to represent the infinite number. 

```javascript
console.log(Number.MAX_VALUE + Number.MAX_VALUE); // Infinity
console.log(-Number.MAX_VALUE - Number.MAX_VALUE); // -Infinity
```

`NaN` stands for Not a Number. It is a special numeric value that indicates an invalid number. The `NaN` has two special characteristics:

* Any operation with `NaN` returns `NaN`.
* The `NaN` does not equal any value, including itself.

```javascript
console.log('a'/2); // NaN;
console.log(NaN/2); // NaN
console.log(NaN == NaN); // false
```

`undefined` type is a special type that has only one value `undefined`.

```javascript
let counter;
console.log(counter);        // undefined
console.log(typeof counter); // undefined
```

In this example, the `counter` is a variable. Since `counter` hasn’t been initialized, it is assigned the value `undefined`. The type of counter is also `undefined`.

For more details, please refer to [this post](https://www.javascripttutorial.net/javascript-data-types/).

# Complex Data Types

In JavaScript, an object is a collection of properties, where each property is defined as a key-value pair. You can access an object's property with `.` or `[]`.

```javascript
let person = {
    firstName: 'John',
    lastName: 'Doe',
    address: {
        city: 'San Jose'
    }
};
console.log(contact.address.city); // "San Jose"
console.log(contact[lastName]);    // "Doe"

// You can also access an object's property using the method as below.
const {firstName, address: {city}} = person;
console.log(city)                  // "San Jose"
```