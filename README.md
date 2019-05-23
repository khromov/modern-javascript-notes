# Modern Javascript course notes
Notes on the Modern Javascript Course by Tyler McGinnis

# Table of contents

<!-- vscode-markdown-toc -->
* 1. [Identifiers](#Identifiers)
* 2. [Variable declarations](#Variabledeclarations)
	* 2.1. [Variable initialization](#Variableinitialization)
	* 2.2. [Scoping](#Scoping)
		* 2.2.1. [Function scoping](#Functionscoping)
		* 2.2.2. [Block scoping](#Blockscoping)
	* 2.3. [Hoisting](#Hoisting)
	* 2.4. [var vs let vs const](#varvsletvsconst)
		* 2.4.1. [Rule of thumb](#Ruleofthumb)
	* 2.5. [Dot notation](#Dotnotation)
	* 2.6. [Object literal notation](#Objectliteralnotation)
	* 2.7. [Destructuring](#Destructuring)
		* 2.7.1. [Renaming properties during destructuring](#Renamingpropertiesduringdestructuring)
		* 2.7.2. [Array destructuring](#Arraydestructuring)
		* 2.7.3. [Glossary: function paremeters vs function arguments](#Glossary:functionparemetersvsfunctionarguments)
	* 2.8. [Destructuring function arguments](#Destructuringfunctionarguments)
		* 2.8.1. [Default values](#Defaultvalues)
		* 2.8.2. [Destructuring pattern for Promise.all](#DestructuringpatternforPromise.all)
	* 2.9. [Shorthand property](#Shorthandproperty)
	* 2.10. [Shorthand method names](#Shorthandmethodnames)
* 3. [Multiline template strings](#Multilinetemplatestrings)

<!-- vscode-markdown-toc-config
	numbering=true
	autoSave=true
	/vscode-markdown-toc-config -->
<!-- /vscode-markdown-toc -->

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


# Reading list
* https://developer.mozilla.org/en-US/docs/Glossary/Hoisting
* https://hackernoon.com/execution-context-in-javascript-319dd72e8e2c
* http://2ality.com/2015/01/es6-destructuring.html

# Useful links
https://www.quora.com/What-is-the-difference-between-arguments-and-parameters-in-JavaScript