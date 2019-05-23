# Modern Javascript course notes
Notes on the Modern Javascript Course by Tyler McGinnis

## Variable declarations

### Variable initialization

Creating the variable

```
var foo;
let bar;
```

### Variable assignment

Assigning the variable

```
foo = 'bar';
```

### Scoping

Can be either block-level or function-level. 

### Hoisting

Variable declaration, assignments and X are hoisted to top of current function scope

### var vs let vs const

* var - function scoped, undefined if used before assigned
* let - block scoped, throws `ReferenceError` if used before assigned
* const - block scoped, throws `ReferenceError` if used before assigned, can't be reassigned once created (properties of the object can still be reassigned)
