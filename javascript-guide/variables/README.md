# [Variables](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Grammar_and_types#variables)
Variables serve as symbolic names for values in a program.
- [Variables](#variables)
  - [Naming](#naming)
  - [Declaration](#declaration)
    - [var](#var)
      - [Basic Usage](#basic-usage)
      - [Scope](#scope)
        - [Unscoped Block Statements](#unscoped-block-statements)
      - [Scoping in Scripts](#scoping-in-scripts)
      - [Scoping in Modules](#scoping-in-modules)
    - [Hoisting](#hoisting)
      - [Hoisting in Scripts](#hoisting-in-scripts)
    - [Redeclarations](#redeclarations)


## Naming
Variable names can start with a letter, underscore (`_`), or dollar sign (`$`). Digits can be included as subsequent characters after the first. 

## Declaration
Variables can be declared using either `var`, `let`, or `const`. `var` can be used to define either a global or local variable depending on the execution context, whereas `let` and `const` define block level variables. 

**Variables should always be declared before being used.**

### [var](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/var)
The var statement declares *function scoped* or *globally scoped* variables.

#### Basic Usage
```
  var x; // variable x declared w/o assignment
  var y = 0; // variable y assigned to 0;
```

#### Scope
The scope of a variable declared using `var` is determined by the most immediate curly bracket syntax fitting one of two types:

* function
* [static initialization block](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/Static_initialization_blocks)

If the variable is not contained within curly braces, its scope is evaluated as either:

* The current module (for code running in module mode)
* The global scope (for code running in script mode)

##### Unscoped Block Statements
Other block statements, including `try...catch`, `switch`, and control flow blocks, **DO NOT** create scopes for `var`. 

Variables declared in these blocks can continue to be referenced outside of these blocks.

#### Scoping in Scripts
In a script, top-level variables declared using `var` are added as non-configurable properties of the [global object](https://developer.mozilla.org/en-US/docs/Glossary/Global_object).

#### Scoping in Modules
In NodeJs CommonJS modules and native ECMAScript Modules, top-level variables are scoped to the module and not added to the global object.

### Hoisting
Var declarations are processed before before execution. Declaring a variable anywhere in the code is equivalent to declaring it at the top of its scope.

A variable can be accessed before its declared, technically, but it will be considered undefined (but declared) until assignment. Assignment statements are **not hoisted**.

#### Hoisting in Scripts
For scripts, var declarations are only hoisted to the top of their respective scripts.

### Redeclarations
STUB