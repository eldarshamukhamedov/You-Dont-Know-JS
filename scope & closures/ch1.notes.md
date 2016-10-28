# You Don't Know JS: Scope & Closures
## Scope

### Summary
1. *Engine*: compiles, maintains scope for, and executes JS code
2. *Compiler*:
    1. *Tokenizing/Lexing:*
        - splits code into tokens, according to language rules
        - "Lexing" is stateful, while "Tokenizing" is stateless.
    2. *Parsing:*
        - generate Abstract Syntax Tree (AST) from array of tokens
    3. *Code-Generation:*
        - convert AST into executable machine code
3. *Scope*:
    - maintains a dictionary of all declared variables
    - regulates code access to those variables
    - can have one parent, but many child *Scopes*
4. *Left-Hand Side look-up*:
    - prior to assignment, determine if a *variable is declared* in *Scope*
    - if variable is not declared:
        - if in *strict mode*, throw `ReferenceError`
        - else, declares variable in outer-most (global) *Scope*
    - Ex. In `var fn = (a) => {}; fn(2);`, LHS look-up for `a`
5. *Right-Hand Side look-up*:
    - retrieve the *value* of a variable, if it exists in *Scope*
    - if variable not found, throws `ReferenceError`
    - Ex. In `var fn = (a) => {}; fn(2);`, RHS look-up for `fn`

### Variable declaration

Given `var a = 1`, *Compiler* sees two statements:
1. `var a;` declaration:
    - if already in current *Scope*, ignore declaration
    - else, declare `a` in given *Scope*
2. `a = 1` assignment:
    - *Compiler* generates code to execute assignment
    - *Engine* attemps to look up varible `a`:
        - if in current *Scope*, assign `2` to `a` in current *Scope*
        - elseif in parent parent *Scope*, assign `2` to `a` in parent *Scope*
        - else (not in any *Scope*), throw `ReferenceError`

### Example: Function declaration and invokation

```js
function foo(a) {
    var b = a;
    return a + b;
}
var c = foo(2);
```

1. LHS look-ups:
    - `c = ...`
    - `(a) {...}`
    - `v = ...`
2. RHS look-ups:
    - ` = foo...`
    - ` = a;`
    - `a + ...`
    - `... + b`

### Example: Nested scope

Given:

```js
function foo(a) {
    console.log(a);
}
foo( 2 );
```

The following occur:
1. *Compiler* declares `foo` on outer *Scope*
2. *Compiler* declares `a` on inner *Scope* (`foo`'s)
3. RHS look-up of `foo`
4. LHS look-up of `a` on inner *Scope*
5. Assign `2` to `a`
6. RHS look-up of `console` on inner *Scope* >> not found
7. RHS look-up of `console` on outer *Scope* >> found!
8. RHS look-up of `log` on inner *Scope* >> not found
9. RHS look-up of `log` on outer *Scope* >> found!
10. RHS look-up of `a` on inner *Scope*
11. Call `log`, passing it value of `a`

### Analogy: Nested scope look-ups as building elevator 

<img src="fig1.png" width="250">

The building represents our program's nested *Scope* rule set. The first floor of the building represents your currently executing *Scope*, wherever you are. The top level of the building is the global *Scope*.

You resolve LHS and RHS references by looking on your current floor, and if you don't find it, taking the elevator to the next floor, looking there, then the next, and so on. Once you get to the top floor (the global *Scope*), you either find what you're looking for, or you don't. But you have to stop regardless.

### Error types

- `ReferenceError` imlies *Scope* resolution-failure
- `TypeError` implies that *Scope* resolution-success, but impossible action attempted against the result

## Review (TL;DR)

Scope is the set of rules that determines where and how a variable (identifier) can be looked-up. This look-up may be for the purposes of assigning to the variable, which is an LHS (left-hand-side) reference, or it may be for the purposes of retrieving its value, which is an RHS (right-hand-side) reference.

LHS references result from assignment operations. *Scope*-related assignments can occur either with the `=` operator or by passing arguments to (assign to) function parameters.

The JavaScript *Engine* first compiles code before it executes, and in so doing, it splits up statements like `var a = 2;` into two separate steps:

1. First, `var a` to declare it in that *Scope*. This is performed at the beginning, before code execution.

2. Later, `a = 2` to look up the variable (LHS reference) and assign to it if found.

Both LHS and RHS reference look-ups start at the currently executing *Scope*, and if need be (that is, they don't find what they're looking for there), they work their way up the nested *Scope*, one scope (floor) at a time, looking for the identifier, until they get to the global (top floor) and stop, and either find it, or don't.

Unfulfilled RHS references result in `ReferenceError`s being thrown. Unfulfilled LHS references result in an automatic, implicitly-created global of that name (if not in "Strict Mode" [^note-strictmode]), or a `ReferenceError` (if in "Strict Mode" [^note-strictmode]).
