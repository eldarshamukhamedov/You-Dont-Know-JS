# You Don't Know JS: Scope & Closures
## Chapter 4: Hoisting

## Chicken Or The Egg?

### Summary

- *Hoisting*:
    - declarations, both variables and functions, are processed first, before any code is executed
    - above effect is called "hoisting"
    - does not apply to function expressions
    - function declarations are hoisted above variable declarations


### Example: hoisted declaration
Given:

```js
console.log(a); // logs undefined, not ReferenceError or 2
var a = 2;
```

`var a` declaration is "hoisted", so `console.log(a)` does not throw `ReferenceError`. This can be thought of as:

```js
// declaration
var a;
// log
console.log( a );
// assignment
a = 2;
```


### Example: function expressions are not hoisted

```js
foo(); // TypeError
bar(); // ReferenceError
var foo = function bar() {};
```

## Review (TL;DR)

We can be tempted to look at `var a = 2;` as one statement, but the JavaScript *Engine* does not see it that way. It sees `var a` and `a = 2` as two separate statements, the first one a compiler-phase task, and the second one an execution-phase task.

What this leads to is that all declarations in a scope, regardless of where they appear, are processed *first* before the code itself is executed. You can visualize this as declarations (variables and functions) being "moved" to the top of their respective scopes, which we call "hoisting".

Declarations themselves are hoisted, but assignments, even assignments of function expressions, are *not* hoisted.

Be careful about duplicate declarations, especially mixed between normal var declarations and function declarations -- peril awaits if you do!
