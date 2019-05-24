# Modern Javascript course notes
Notes on the Modern Javascript Course by Tyler McGinnis

# Table of contents

- [Modern Javascript course notes](#modern-javascript-course-notes)
- [Table of contents](#table-of-contents)
- [Assigning variables, functions and properties](#assigning-variables-functions-and-properties)
  - [Identifiers](#identifiers)
  - [Variable declarations](#variable-declarations)
    - [Variable initialization](#variable-initialization)
    - [Scoping](#scoping)
      - [Function scoping](#function-scoping)
      - [Block scoping](#block-scoping)
    - [Hoisting](#hoisting)
    - [var vs let vs const](#var-vs-let-vs-const)
      - [Rule of thumb](#rule-of-thumb)
- [Object and Array destructuring](#object-and-array-destructuring)
    - [Dot notation](#dot-notation)
    - [Object literal notation](#object-literal-notation)
    - [Destructuring](#destructuring)
      - [Renaming properties during destructuring](#renaming-properties-during-destructuring)
      - [Array destructuring](#array-destructuring)
      - [Glossary: function paremeters vs function arguments](#glossary-function-paremeters-vs-function-arguments)
    - [Destructuring function arguments](#destructuring-function-arguments)
      - [Default values](#default-values)
      - [Destructuring pattern for Promise.all](#destructuring-pattern-for-promiseall)
- [Shorthand property and method names](#shorthand-property-and-method-names)
    - [Shorthand property](#shorthand-property)
    - [Shorthand method names](#shorthand-method-names)
- [Computed property names](#computed-property-names)
- [Template literals (template strings)](#template-literals-template-strings)
  - [Multiline template strings](#multiline-template-strings)
- [Arrow functions](#arrow-functions)
  - [Implicit return](#implicit-return)
  - [`this` keyword](#this-keyword)
  - [Implicit returns with objects / multiple lines](#implicit-returns-with-objects--multiple-lines)
      - [debugging implicit return](#debugging-implicit-return)
- [Default parameters](#default-parameters)
  - [Use function to validate default parameters](#use-function-to-validate-default-parameters)
- [Compiling versus Polyfills with Babel](#compiling-versus-polyfills-with-babel)
- [import, export and modules](#import-export-and-modules)
  - [Import all exports](#import-all-exports)
  - [Default exports](#default-exports)
  - [Mixing default and named exports](#mixing-default-and-named-exports)
- [async / await](#async--await)
- [Reading list](#reading-list)
- [Useful links](#useful-links)

# Assigning variables, functions and properties

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

Scoping can either be block-level or function-level. 

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

Variable and function declaration are hoisted to top of current scope. 
Only variable declarations are hoisted, not initializations.

```js
console.log(num); // undefined, because declaration was hoisted but not initialization
var num; // declaration
num = 6; // initialization
```

```js
catName("Tigger"); // "My cat's name is Tigger", because function declaration was hoisted to top.

function catName(name) {
  console.log("My cat's name is " + name);
}
```

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

# Object and Array destructuring

### Dot notation 

Adds properties to object:

```js
const user = {};
user.name = 'Timmy';
console.log(user.name); //Timmy
```

### Object literal notation 

Adds multiple properties when creating an object:

```js
const user = {
  name: 'Timmy'
}
console.log(user.name); //Timmy
```

### Destructuring

Allows us to extract multiple properties from an object or result of function invocation.

```js
const user = {
  name: 'Timmy',
  handle: '@T'
}

let { name, handle } = user;

console.log(name); // "Timmy"
console.log(handle); // "@T"
```

#### Renaming properties during destructuring

```js
const user = {
  n: 'Timmy',
  h: '@T',
  l: 'Stockholm',
}

const { n: name, h: handle, l: location } = user; 
console.log(name, handle, location); //"Timmy" "@T" "Stockholm" 
```


#### Array destructuring

```js
const user = ['Timmy', '@T', 'Stockholm'];
const [name, handle, location] = user;
console.log(name, handle, location); //"Timmy" "@T" "Stockholm"
```

#### Glossary: function paremeters vs function arguments

```js
let foo = function(a, b) { /*...*/ } //a and b are parameters

foo('c','d') //'c' and 'd' are function arguments
```

### Destructuring function arguments

Useful when a function takes many arguments. Lets you ignore the order of arguments. 

```js
//Function with many arguments
let foo = function(a,b,c,d) { /*...*/ }

//Calling it requires setting all parameters in order
foo('a','b','c','d');

//Better destructured version
let bar = function({ a, b, c, d }) { /*...*/ }
bar({ a: 'a', b: 'b', c: 'c', d: 'd' }); //Argument order not important
```

#### Default values

```js
let baz = function({ a = 'a', b = 'b' }) { // Sets default values for a and b
  console.log(a, b);
}
baz({ b: 'BBB' }); // 'a' (default), 'BBB' (passed)
```

#### Destructuring pattern for Promise.all

```js
function getUserData(user) {
  return Promise.all([
    getName(user),
    getHandle(user),
  ]).then(function(['name', 'handle']) {
    return {
      name: name,
      handle: handle
    }
  })
}
```

Shortened best-practices version of above:

```js
function getUserData(user) {
  return Promise.all([
    getName(user),
    getHandle(user),
  ]).then((['name', 'handle']) => (name, handle)
}
```

# Shorthand property and method names

### Shorthand property

If the property key of an object is the same as the value assigned to that property, you can omit the colon and value.

```js
function formatMessage(foo, bar) {
  return {
    foo: foo,
    bar: bar
  }
}

// Same as above
function formatMessageNew(foo, bar) {
  return {
    foo,
    bar
  }
}
```

### Shorthand method names

A **function** that is the **property of an object** is called a **method**.

```js
function foo() {
  return {
    bar: function() {
      /*...*/
    }
  }
}

// Same as above 
function foo() {
  return {
    bar() {
      /*...*/
    }
  }
}
```

# Computed property names

```js
function objectify (key, value) {
  let obj = {}
  obj[key] = value
  return obj;
}

// Same as above
function objectify(key, value) {
  return {
    [key]: value
  }
}

objectify('kitty', 27);
```

# Template literals (template strings)

```js
function welcomeMessage(firstName, lastName, email) {
  return `Hello ${firstName} ${lastName}. Your email is ${email}`;
}

welcomeMessage('John', 'Doe', 'john@doe.com');
```

## Multiline template strings

```js
function welcomeMessage() {
  return `
          <span>
            Nice to meet you!
          </span>
  `
}
welcomeMessage()
```

# Arrow functions

* More terse, less typing
* Implicit return (doesn't need `return` statement if written on same line)
* `this` keyword TODO

```js
// Function expression
const add =  function(x,y) {
  return x + y;
}

//Same as above
const addArrow = (x, y) => {
  return x + y;
}
```

## Implicit return

```js
const getTweets = (tweets) => tweets.response();

// Same as above, works only if function has one parameter
const getTweets = tweets => tweets.response();
```

## `this` keyword

Arrow functions **do not create a new function context**. They share `this` from the parent scope.

## Implicit returns with objects / multiple lines

```js
this.getUsers((users) => ({
  users
}))
```

#### debugging implicit return

```js
this.getUsers((users) => console.log(users) || ({
  users
}))
```

# Default parameters

```js
function foo(bar = 'baz') { /*...*/ }
```

## Use function to validate default parameters

```js
function isRequired(name) {
  throw new Error(name + ' is required');
}

function calculatePayment(price = isRequired('price'), salesTax = 0.0047) {
 // Math
}

calculatePayment(); // Uncaught Error: price is required 
```

# Compiling versus Polyfills with Babel

Babel transforms the syntax of your code to transform it into compatible syntax for older browsers. It does not polyfill any features, like `Fetch` or `Promise`.

Compiling doesn't add new properties to any primitives.

# import, export and modules

```js
// math.js
export function add (x,y) {
  return x + y
}
export function multiply (x,y) {
  return x * y
}
export function divide (x,y) {
  return x / y
}
// main.js
import { add, multiply } from './math'
add(1,2) // 3
multiply(3,4) // 12
```

## Import all exports

```js
// math.js (same as above)
// main.js
import * as math from './math'
math.add(1,2) // 3
math.multiply(3,4) // 12
math.divide(4,4) // 1
```

## Default exports


```js
// math.js
export default function doAllTheMath (x,y,z) {
  return x + y + x * x * y * z / x / y / z
}
// main.js
import doAllTheMath from './math'
doAllTheMath(1,2,3) // 4
```


## Mixing default and named exports

```js
// math.js
export function add (x,y) {
  return x + y
}
export default function doAllTheMath (x,y,z) {
  return x + y + x * x * y * z / x / y / z
}
// main.js
import doAllTheMath, { add } from './math'
doAllTheMath(1,2,3) // 4
add(1,2) // 3
```

```js
import { Route, Link, Router } from 'react-router';
```

# async / await

Every async function you write will return a promise, and every single thing you await will ordinarily be a promise.

```js
function getUser () {
  return new Promise((resolve, reject) => {
    setTimeout(() => resolve({name: 'Tyler'}), 2000)
  })
}

async function handleGetUser () {
  try {
    var user = await getUser()
    console.log(user)
  } catch (error) {
    console.log('Error in handleGetUser', error)
  }
}
```

* https://learn.tylermcginnis.com/courses/51206/lectures/816838
* https://medium.com/@bluepnume/learn-about-promises-before-you-start-using-async-await-eb148164a9c8
* https://pouchdb.com/2015/05/18/we-have-a-problem-with-promises.html


# Reading list
* https://developer.mozilla.org/en-US/docs/Glossary/Hoisting
* https://hackernoon.com/execution-context-in-javascript-319dd72e8e2c
* http://2ality.com/2015/01/es6-destructuring.html

# Useful links
https://www.quora.com/What-is-the-difference-between-arguments-and-parameters-in-JavaScript