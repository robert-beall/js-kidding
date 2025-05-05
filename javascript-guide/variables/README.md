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
        - [Redeclaration without Reassignment](#redeclaration-without-reassignment)
        - [Redeclaration with Reassignment](#redeclaration-with-reassignment)
        - [Function Declarations](#function-declarations)
        - [Interactions with `let`, `const`, `class`, and `import`](#interactions-with-let-const-class-and-import)
        - [Within a Function Body](#within-a-function-body)
        - [Within a Catch Block](#within-a-catch-block)
    - [let](#let)
      - [Basic Usage](#basic-usage-1)
      - [Scope](#scope-1)
        - [Curly-Braced-Syntax](#curly-braced-syntax)
        - [Outside of Curly Braces](#outside-of-curly-braces)
      - [Differences with Var](#differences-with-var)
      - [Temporal Dead Zone (TDZ)](#temporal-dead-zone-tdz)


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

##### Scoping in Scripts
In a script, top-level variables declared using `var` are added as non-configurable properties of the [global object](https://developer.mozilla.org/en-US/docs/Glossary/Global_object).

##### Scoping in Modules
In NodeJs [CommonJS](https://nodejs.org/docs/latest/api/modules.html#modules-commonjs-modules) modules and native [ECMAScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules) Modules, top-level variables are scoped to the module and not added to the global object.

#### Hoisting
Var declarations are processed before before execution. Declaring a variable anywhere in the code is equivalent to declaring it at the top of its scope.

A variable can be accessed before its declared, technically, but it will be considered undefined (but declared) until assignment. Assignment statements are **not hoisted**.

##### Hoisting in Scripts
For scripts, var declarations are only hoisted to the top of their respective scripts.

#### Redeclarations
Duplicate declarations using `var` will not trigger an error, even in [strict mode](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode). The variable will not lose its value, unless the declaration has an assignment. 

##### Redeclaration without Reassignment
```
var a = 2;
var a; // a = 2
```

##### Redeclaration with Reassignment
```
var a = 2;
var a = 5; // now a = 5
```

##### Function Declarations
`var` declarations in the same scope as a function declarations take precedent, overriding the function declaration.

This happens because function declarations are hoisted above any variable declarations, causing the later variable initializer to overwrite the function.

```
var a = 2;

function a(i) {
  return i+1;
}

console.log(a); // a = 2
```

##### Interactions with `let`, `const`, `class`, and `import`
`var` declarations **cannot** be in the same scope as `let`, `const`, `class`, and `import` declarations.

```
var a;
let a; <-- This throws an error
```

Remember that `var` declarations are not scoped to blocks, making this invalid as well:

```
let a = 1;

{
  var a = 2;
}
```

However, the reverse is allowable because `let` is scoped to a block: 

```
var a = 1;

{
  let a = 2;
}
```

##### Within a Function Body
A var declaration within a function body can have the same name as a parameter, in which case it will override the parameter value:

```
function log(a) {
  var a = 1;
  console.log(a);
}

log(2); // will log '1'
```

##### Within a Catch Block
A var declaration within a catch block can have the same name as the catch-bound identifier, but only if the catch binding is a simple identifier, not a destructuring pattern. In this case, the var declaration is hoisted above the catch block, but any value assigned in the catch block is not visible outside of it.

```
try {...}
catch (e) {
  var e = 2; // allowable
}

console.log(e); // will log 'undefined'
```

### [let](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let)
`let` is used to declare *block-scoped* local variables.

#### Basic Usage
```
let a = 1;

{
  let a = 2;
  console.log(a); // 2
}

console.log(a); // 1
```

#### Scope
##### Curly-Braced-Syntax
A `let` declared variable will be scoped to the most immediate following syntax that encloses it:
- `block` Statement 
- `switch` Statement
- `try...catch` Statement
- Control flow statements (`for`, `while`, etc.) if the `let` is in the header of the statement.
- Function Body
- Static Initialization Block 
##### Outside of Curly Braces 
If a `let` statement is not constrained to a curly braced syntax, it is scoped to one of the following that applies:
- If in module mode, the **current module**
- If in script mode, the **global scope**
#### Differences with Var
- Scoped to **blocks** as well as functions
- Can only be accessed **after** the place of declaration is reached (commonly considered **non-hoisted**).
- **Do not** create properties on `globalThis` when declared at the top-level of a script
- **Cannot be redeclared** in the same scope level.
- You cannot use a let as a lone statement in the body of a block
  - This is pointless anyways as the declared variable can never be accessed.
#### Temporal Dead Zone (TDZ)
- 
