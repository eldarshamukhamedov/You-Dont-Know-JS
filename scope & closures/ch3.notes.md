# You Don't Know JS: Scope & Closures

## Chapter 3: Function vs. Block Scope

### Summary

1. anonymous functions drawbacks:
    - no name in stack traces
    - cannot self-reference
2. Immediately Invoked Function Expression (IIFE)
3. *Principle of Least Privilege*
3. *Block scope*:
    - declare variables as close to use/as locally as possible
    - `with()` is a form of block scoping
    - `catch` clause in `try {} catch {}` is block-scoped
    - `let` and `const`
        - `let` declares variables in current *block scope*
        - `const` declares constants in current *block scope*
        - neither `let` nor `const` is hoisted
    - explicit `{}` blocks

### Example: IIFE to restore initial undefined value
```js
undefined = true; // setting a land-mine for other code! avoid!
(function IIFE( undefined ){
    var a;
    if (a === undefined) console.log( "Undefined is safe here!" );
})(/* nothing */);
```

### Example: `try {} catch {}` block scope

```js
try {
    throw new Error('uh oh');
} catch (err) {
    console.log(err); // Error('uh oh')
}
console.log(err); // ReferenceError: `err` not found
```

### Example: explicit block scope declaration
```js
{ // <-- explicit block
    let foo = 'bar';
    foo = something(foo);
    console.log(foo); // "bar"
}
console.log(foo); // ReferenceError: `foo` not found
```

### Example: block scope variables are not hoisted
```js
{
   console.log(foo); // ReferenceError: `foo` not found
   let foo = 2;
}
```

## Review (TL;DR)

Functions are the most common unit of scope in JavaScript. Variables and functions that are declared inside another function are essentially "hidden" from any of the enclosing "scopes", which is an intentional design principle of good software.

But functions are by no means the only unit of scope. Block-scope refers to the idea that variables and functions can belong to an arbitrary block (generally, any `{ .. }` pair) of code, rather than only to the enclosing function.

Starting with ES3, the `try/catch` structure has block-scope in the `catch` clause.

In ES6, the `let` keyword (a cousin to the `var` keyword) is introduced to allow declarations of variables in any arbitrary block of code. `if (..) { let a = 2; }` will declare a variable `a` that essentially hijacks the scope of the `if`'s `{ .. }` block and attaches itself there.

Though some seem to believe so, block scope should not be taken as an outright replacement of `var` function scope. Both functionalities co-exist, and developers can and should use both function-scope and block-scope techniques where respectively appropriate to produce better, more readable/maintainable code.

[^note-leastprivilege]: [Principle of Least Privilege](http://en.wikipedia.org/wiki/Principle_of_least_privilege)
