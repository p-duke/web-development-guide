<!-- @format -->

## Table of Contents

### Javascript

- [Hoisting](#hoisting)

### Hoisting

A general definition of hoisting is that its a JavaScript mechanism where variables and function declarations are
moved to the top of their scope before code execution.<sup>[2]</sup> This is not in fact what happens. Instead, the variable and function declarations are put into memory during the compile phase, but stay exactly where you typed them in your code.<sup>[1]</sup>

- Only the actual declarations are hoisted
- Any assignments are left where they are
- A declaration is hoisted with the `undefined` value
- One of the advantages is it allows you to use a function before you declare it in your code
- Javascript only hoists declarations not initializations<sup>[1]</sup>

Function Hoisting

- Function declarations are also hoisted but functions that are assigned values
  are not hoisted

### Example

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
x = 1
console.log(x + ' ' + y); // '1 undefined'
var y = 2;
```

### Sources
[1]: https://developer.mozilla.org/en-US/docs/Glossary/Hoisting "MDN Web Docs"
[MDN Web Docs](https://developer.mozilla.org/en-US/docs/Glossary/Hoisting)

[2]: https://www.sitepoint.com/back-to-basics-javascript-hoisting/ "Back To Basics"
[Back To Basics](https://www.sitepoint.com/back-to-basics-javascript-hoisting/)

