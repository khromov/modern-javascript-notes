# Modern Javascript course notes
Notes on the Modern Javascript Course by Tyler McGinnis

## Identifiers

A name that identifies a variable, function, or property. 

Identifiers are case-sensitive and can contain Unicode letters, $, \_, and digits (0-9), but may not start with a digit.

## Variable declarations

Introduces a new identifier.

Creating the variable

```js
var foo;
let bar;
```

### Variable initialization

Assigning a value to the variable

```js
foo = 'bar';
```

### Scoping

JavaScript is generally function scoped. Only functions introduce new scopes.

Scoping can either be block-level or function-level. Only declarations are hoisted, not initializations.

```js
console.log(num); // undefined, because declaration was hoisted but not initialization
var num; // declaration
num = 6; // initialization
```


#### Function scoping

```js
// Global Scope
var firstFunction = function () {
  // firstFunction's Scope
  var secondFunction = function () {
    // secondFunction's Scope
  };
};
```

#### Block scoping

Everything between two `{ }` is a new block scope. e.g. `while`, `for`, `if`.

### Hoisting

Variable declaration, function and variable declarations are hoisted to top of current  scope. 

### var vs let vs const

* var - function scoped, undefined if used before assigned
* let - block scoped, throws `ReferenceError` if used before assigned
* const - block scoped, throws `ReferenceError` if used before assigned, can't be reassigned once created (properties of the variable can still be modified), ie:

```js
const cat = {
  name: 'Fluffy'
};

cat = {}; // Uncaught TypeError: Assignment to constant variable

cat.name = 'Whiskers' = // OK, because we're modifying a property, not reassigning object
```

#### Rule of thumb 
Always use `const`, unless variable might be reassigned later. In those cases use `let`. Never use `var`.



# Reading list
https://developer.mozilla.org/en-US/docs/Glossary/Hoisting
https://hackernoon.com/execution-context-in-javascript-319dd72e8e2c
