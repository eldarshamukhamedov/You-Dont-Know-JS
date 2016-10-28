# You Don't Know JS: Scope & Closures
## Lexical Scope

### Summary

1. *Lexical scope* is:
    - defined at lexing time
    - based on where variables and blocks of scope are authored by programmer
    - (mostly) immutable once lexed
    - does not apply to object property look-ups
2. Cheating *lexical scope*:
    - `eval()` and `new Function()` constructor can modify scopes they were executed in
    - strict mode `eval` spawn own lexical scope
    - `with` (deprecated) treats passed object as a scope and its shallow keys as identifiers
    - JS engines typically disable all optimizations  

### Example: Nested lexical scope
Given:
```js
function foo(a) {
    var b = a * 2;
    function bar(c) {
        console.log(a, b, c);
    }
    bar(b * 3);
}
foo(2); // 2 4 12
```
The following scopes exist:
<img src="fig2.png" width="500">

1. global scope with just one identifier: `foo`
2. scope of `foo` with three identifiers: `a`, `bar`, `b`
3. scope of `bar` with just one identifier: `c`

## Review (TL;DR)

Lexical scope means that scope is defined by author-time decisions of where functions are declared. The lexing phase of compilation is essentially able to know where and how all identifiers are declared, and thus predict how they will be looked-up during execution.

Two mechanisms in JavaScript can "cheat" lexical scope: `eval(..)` and `with`. The former can modify existing lexical scope (at runtime) by evaluating a string of "code" which has one or more declarations in it. The latter essentially creates a whole new lexical scope (again, at runtime) by treating an object reference *as* a "scope" and that object's properties as scoped identifiers.

The downside to these mechanisms is that it defeats the *Engine*'s ability to perform compile-time optimizations regarding scope look-up, because the *Engine* has to assume pessimistically that such optimizations will be invalid. Code *will* run slower as a result of using either feature. **Don't use them.**
