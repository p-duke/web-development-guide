<!-- @format -->

## Table of Contents

### Javascript

- [Hoisting](#hoisting)

### Hoisting

A general definition of hoisting is that its a JavaScript mechanism where
variables and function declarations are moved to the top of their scope before
code execution.<sup>[2]</sup> This is not in fact what happens. Instead, the
variable and function declarations are put into memory during the compile phase,
but stay exactly where you typed them in your code.<sup>[1]</sup>

TLDR;

- Javascript only hoists the actual declarations but initializations or
  assignments are left where they are<sup>[1]</sup>
- A declaration is hoisted with the `undefined` value
- One of the advantages is it allows you to use a function before you declare it
  in your code are not hoisted
- undeclared variable is assigned the value undefined at execution and is also
  of type undefined.<sup>[3]</sup>
- ReferenceError is thrown when trying to access a previously undeclared
  variable.<sup>[3]</sup>
- variable declarations are processed before any code is executed.<sup>[3]</sup>
- Assigning a value to an undeclared variable implicitly creates it as a global
  variable when the assignment is executed. This means that, all undeclared
  variables are global variables.<sup>[3]</sup>
- In the es5 version of Javascript we can use _strict mode_ which will not
  tolerate usage of variables before declared<sup>[3]</sup>
- In es6 we can now use `let` and `const` which also prohibits the use of an
  undeclared variable
- Variables declared with `let` and `const` remain uninitialised at the
  beginning of execution whilst variables declared with var are initialised with
  a value of _undefined_.<sup>[3]</sup>

Function Hoisting

- Javascript functions can be loosly classified as a `declaration` or
  `expression`
- Function declarations are also hoisted but functions that are assigned values

Order of Precedence:

- Variable assignment takes precendence over function declaration<sup>[3]</sup>
- Function declarations take precendence over variable
  declarations<sup>[3]</sup>
- Function declarations are hoisted over variable declarations but not over
  variable assignments.<sup>[3]</sup>

Recommendations

- Always declare variables (using `var`, `let`, `const`) regardless of global or
  function scope.
- Make sure to declare and initialize a variable before using it.
- Enable the use of _strict mode_ with es5 by using `"use strict"` at the top of
  the file can help expose undeclared variables.

### Examples

Variable assignment over function declaration:

```javascript
var double = 22;

function double(num) {
  return num * 2;
}

console.log(typeof double); // Output: number
```

Function declaration over variable declarations:

```javascript
var double;

function double(num) {
  return num * 2;
}

console.log(typeof double); // Output: function
```

A function declaration is hoisted and can therefore be called before declared:

```javascript
hoisted(); // Output: I've been hoisted!

function hoisted() {
  console.log('I've been hoisted!');
}
```

A function expression is not hoisted and cannot be called before declared:

```javascript
expression(); // Output: "TypeError: expression is not a function

var expression = function() {
  console.log('Will this work?');
};
```

Usage of `let` in es6 will not allow use of undeclared variables:

```javascript
console.log(hoist); // Output: ReferenceError: hoist is not defined
let hoist = 'The variable has been hoisted.';
```

With `const` the same rules except initialization must occur as well:

```javascript
const PI;
console.log(PI); // Ouput: SyntaxError: Missing initializer in const declaration
PI=3.142;

function getCircumference(radius) {
  console.log(circumference)
  circumference = PI*radius*2;
  const PI = 22/7;
}

getCircumference(2) // ReferenceError: circumference is not defined
```

Assigning a value to an undeclared variable implicitly puts it in the global
scope.

```javascript
function hoist() {
  a = 20;
  var b = 30;
}

hoist();

console.log(a); // prints out 20
console.log(b); // Throws a ReferenceError
```

Declaration, initialization, assignment...

```javascript
var a; // declaration
a = 100; // initialization, assignment. Initialization also causes declaration (if not already declared), variables are available.

var a = 100; // usage
a + 30;
```

An IIFE like this:

```javascript
(function() {
  var foo = 1;
  console.log(foo + ' ' + bar + ' ' + baz);
  var bar = 2;
  var baz = 3;
})();
```

Translates to the following code:

```javascript
(function() {
  var foo;
  var bar;
  var baz;

  var foo = 1;
  console.log(foo + ' ' + bar + ' ' + baz);
  var bar = 2;
  var baz = 3;
})();
```

This function will behave as expected due to function hoisting:

```javascript
foo();

function foo() {
  console.log('Hello');
}
```

This function will throw an exception since only the variable declaration of foo
is hoisted _NOT_ the assignment:

```javascript
foo();

var foo = function() {
  console.log('Hello');
};
```

Only `y` is hoisted since declared but x is not hoisted:

```javascript
x = 1;
console.log(x + ' ' + y); // '1 undefined'
var y = 2;
```

### Sources

[1]: https://developer.mozilla.org/en-US/docs/Glossary/Hoisting 'MDN Web Docs'

[MDN Web Docs](https://developer.mozilla.org/en-US/docs/Glossary/Hoisting) [2]:
https://www.sitepoint.com/back-to-basics-javascript-hoisting/ "Back To Basics"
[Back To Basics](https://www.sitepoint.com/back-to-basics-javascript-hoisting/)
[3]: https://scotch.io/tutorials/understanding-hoisting-in-javascript
"Understanding Hoisting in Javascript"
[Understanding Hoisting in Javascript](https://scotch.io/tutorials/understanding-hoisting-in-javascript)
