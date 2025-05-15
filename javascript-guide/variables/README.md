# [Grammar and Types](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Grammar_and_types#variables)
Variables serve as symbolic names for values in a program.
- [Grammar and Types](#grammar-and-types)
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
      - [Redeclarations](#redeclarations-1)
      - [Top Level Declarations](#top-level-declarations)
      - [Scoping Rules](#scoping-rules)
      - [TDZ and Lexical Scoping](#tdz-and-lexical-scoping)
      - [Declaration with destructuring](#declaration-with-destructuring)
    - [const](#const)
      - [Syntax](#syntax)
      - [Similarities with `let`](#similarities-with-let)
      - [Unique qualities of `const`](#unique-qualities-of-const)
        - [Immutable Binding, Not Immutable Value](#immutable-binding-not-immutable-value)
        - [Required Initialization](#required-initialization)
        - [When to use `const`](#when-to-use-const)
        - [Binding](#binding)
      - [`const` with objects and arrays](#const-with-objects-and-arrays)
  - [Data structures and types](#data-structures-and-types)
    - [Primitive Types](#primitive-types)
    - [Objects](#objects)
      - [Keys](#keys)
    - [Functions](#functions)
    - [Dynamic Typing and Data Conversion](#dynamic-typing-and-data-conversion)
    - [Numbers, Strings and the `+` operator](#numbers-strings-and-the--operator)
    - [Converting Strings to Numbers](#converting-strings-to-numbers)
      - [parseInt(...)](#parseint)
        - [Parameters:](#parameters)
        - [Return Value](#return-value)
      - [parseFloat(...)](#parsefloat)
        - [Parameters](#parameters-1)
        - [Return Value](#return-value-1)
      - [Number(...)](#number)
        - [Parameter](#parameter)
        - [Return Value](#return-value-2)
      - [Unary +](#unary-)
  - [Literals](#literals)
    - [Array Literals](#array-literals)
      - [Extra Commas](#extra-commas)
        - [Trailing Commas and Git Diffs](#trailing-commas-and-git-diffs)
        - [Best Practice](#best-practice)
    - [Boolean Literals](#boolean-literals)
    - [Numeric Literals](#numeric-literals)
      - [Integer Literals](#integer-literals)
      - [Floating Point Literals](#floating-point-literals)
    - [Object Literals](#object-literals)
    - [RegExp Literals](#regexp-literals)
    - [String Literals](#string-literals)


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
Var declarations are processed before execution. Declaring a variable anywhere in the code is equivalent to declaring it at the top of its scope.

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
Variables declared with let
- Are scoped to **blocks** as well as functions
- Can only be accessed **after** the place of declaration is reached (commonly considered **non-hoisted**).
- **Do not** create properties on `globalThis` when declared at the top-level of a script
- **Cannot be redeclared** in the same scope level.
- You cannot use a let as a lone statement in the body of a block
  - This is pointless anyways as the declared variable can never be accessed.

#### Temporal Dead Zone (TDZ)
A variable declared with `let`, `const`, or `class` will throw a [ReferenceError](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/ReferenceError) if accessed before it has been initialized with a value. The section of code from the start of a block to where the variable has been declared and initialized is known as the Temporal Dead Zone(TDZ). 

The variable is initialized with a value once the declaration has been reached in the code. 

Meanwhile, variables declared with var will be `undefined` when accessed before declaration.

This behavior can be viewed in the code snippet below:

```
{
    /** Temporal Dead Zone */
    console.log(a); // undefined
    console.log(b); // ReferenceError
    /** End TDZ */

    var a;
    let b; 

    // After Declaration, Before Initialization
    console.log(b); // undefined
    b = 2;
    // After Declaration, After Initialization
    console.log(b); // 2
}
```

The TDZ is "temporal" because it is determined by execution time, not placement in the code. The following code will not throw a reference error:

```
{ // TDZ starts at beginning of scope
  /** function f defined in TDZ but not called. */
  const f = () => console.log(l); 
  
  /** TDZ ends after 'l' declared and initialized. */
  let l = 'A';
  f(); // function call comes after TDZ ends.
} 
```

The `typeof` operator will throw a reference error for a variable in its TDZ. By contrast, using `typeof` on undeclared variables will result in `undefined`.

#### Redeclarations
`let` declarations cannot be in the same scope as any other declaration (`let`, `const`, `class`, `function`, `var`, `import`).

Unlike with `var`, a `let` declaration inside a function cannot share the same name as a parameter. Likewise, a `let` declaration in a catch block cannot have the same name as the catch-bound identifier. 

```
function f(a) {
  let a = 'b'; // Throws a SyntaxError
}

try {
  ...
} catch (e) {
  let e; // Also throws a SyntaxError
}
```

Errors may also be encountered with switch statements, which encompass a single block by default: 

```
switch (a) {
  case 0: 
    let b;
    break;
  case 1:
    let b; // SyntaxError (b already declared in the switch scope)
    break
}
```

To fix this issue, wrap each case in its own block:

```
switch (a) {
  case 0: {
    let b;
    break;
  }
  case 1: {
    let b; // no error
    break; 
  }
}
```

#### Top Level Declarations
At the top level of programs and functions, `let` does not create a property on the global object, while `var` does.

```
var a = 'test';
let b = 'testing';

console.log(this.a); // 'test'
console.log(this.b); // undefined
```

#### Scoping Rules
Whereas variables declared using `var` are scoped to an enclosing function, variables declared using `let` are scoped to the nearest block. For example:

```
// 'a' is the same variable throughout.
function withVar() {
  var a = 0;

  {
    var a = 1;
    console.log(a); // 1
  }

  console.log(a); // 1
}
```

With let, each new `let` declaration in a nested block is treated as a different variable.

```
function withTest() {
  let b = 0;

  {
    let b = 1; // this declares a new variable
    console.log(b); // 1
  }

  console.log(b); // 0 (the first b declared in the outer scope)
}
```

#### TDZ and Lexical Scoping
A declaration using `let` supersedes any definition in an outer scope in the block it is defined within. This can lead to unexpected `ReferenceError`s. 

For example:

```
var x = 1;

if (x) {
  let x = x + 20; // ReferenceError
}
```

In this example, the `let` declared `x` supersedes the `var` declaration within the if statement, and thus the arithmetic on `x` involves an illegal access of the `let` defined variable within its temporal dead zone. 

A similar situation can occur with control flow statements:

```
var arr = { x: [1, 2, 3] };

for (let arr of arr.x) { // ReferenceError
  console.log(arr);
}
```

The `let` declared `arr` is scoped to the for-block.  

#### Declaration with destructuring
The left side of a `=` can be a [binding pattern](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring#binding_and_assignment), allowing for the creation of multiple variables at once.

```
const arr = ['a', 'b', 'c'];

let [a, b, c] = result;

console.log(b); // 'b'
```

### const
The `const` declaration is used to declare constant, block-scoped local variables. A `const` is like a `let` that cannot be changed after initial assignment.

**NOTE:** If a constant is an object, its properties can be added, updated, or removed. 

#### Syntax 
```
// s cannot be reassigned to a different value
const s = 'foo'; 
```

#### Similarities with `let`
const declarations 

- are scoped to blocks and functions
- Have a temporal dead zone
- do not create properties on [globalThis](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/globalThis) when declared at the top level of a script.
- cannot be redeclaredin the same scope.
- cannot be used as the sole body of a block (since there is no way to access the const)
  ```
  if (a) const b = 1; // SyntaxError
  ```

#### Unique qualities of `const`

##### Immutable Binding, Not Immutable Value
A constant creates an *immutable* reference to a value. The value referenced, however, is not immutable and can be changed. It **creates an immutable binding, not an immutable value.**

##### Required Initialization
A `const` declared variable **requires initialization**.

```
// Correct
const B = 'b';

// Incorrect (Throws a SyntaxError)
const A;  
```

##### When to use `const`
It is generally considered best practice to use `const` over `let` when a variable is not going to be reassigned in its scope. This makes the intent surrounding the variable clear and prevents unintended changes to the variable. 

##### Binding
The list that follows the `const` keyword is called a [binding list](https://developer.mozilla.org/en-US/docs/Glossary/Binding). It is separated by commas (which are not the [comma operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Comma_operator)) and the `=` sign (which is not an [assignment operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Assignment)). This can be a little confusing, so just remember that the `const` declarator creates a **reference**, not a value.

Initializers of later variables can refer to earlier variables in the binding list. 

#### `const` with objects and arrays
You cannot reassign a constant initialized to an object or array: 

```
const OBJ = { a: '1', b: '2' };

OBJ = { c: '3' };
```

This **DOES NOT** make referenced object or array immutable. The following is allowed:

```
const OBJ = { a: '1' };

OBJ.b = '2';
```

To make an object immutable, you must use [`Object.freeze(...)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze).

The same applies to arrays:

```
const ARR = [];

// illegal
ARR = [1];

// allowed
ARR.push(2);
```

## Data structures and types
ECMAScript standard defines 8 data types.

### [Primitive Types](https://developer.mozilla.org/en-US/docs/Glossary/Primitive)
A **primitive** is data that is *not an object* and has no *methods* or *properties*. 

Primitives are immutable, meaning their values cannot be changed. A variable may be reassigned to a different value, but the underlying value cannot be altered.

Primitives have no methods, but behave as if they do. This is because JavaScript *auto-boxes* the value into a wrapper object. Any methods or properties accessed from a primitive are actually accessed from the wrapping object. 

The primitive types in JavaScript are:

1. [Boolean](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Grammar_and_types#variables:~:text=are%20primitives%3A-,Boolean,-.%20true%20and): `true` or `false`
2. [null](https://developer.mozilla.org/en-US/docs/Glossary/Null): special keyword denoting a null value. **NOTE:** JavaScript is case-sensitive, and null must be all undercase. 
     - **Fun Fact:** while `null` is a primitive, using `typeof` on it returns "object". This is a bug, but it cannot be fixed because too many scripts would be broken by it.
3. [undefined](https://developer.mozilla.org/en-US/docs/Glossary/Undefined): A primitive assigned to variables that have been declared, but not initialized, or to formal function arguments for which there are no actual arguments:
    ```
      let a;
      console.log(a); // undefined

      function x(y) {
        if (typeof y == 'undefined') {
          return 1;
        } 

        return 0;
      }

      /* 
       * function x takes an argument, but we don't want to
       * pass in an actual value.
       */
      x(undefined); 
    ```
4. [Number](https://developer.mozilla.org/en-US/docs/Glossary/Number): integer or floating point number. In JavaScript, all numbers are in the [double-precision 64-bit floating point format](https://en.wikipedia.org/wiki/Double_precision_floating-point_format).
5. [BigInt](https://developer.mozilla.org/en-US/docs/Glossary/BigInt): integer with [arbitrary precision](https://en.wikipedia.org/wiki/Arbitrary-precision_arithmetic) (e.g. `900000342143899n`).
6. [String](https://developer.mozilla.org/en-US/docs/Glossary/String): sequence of text characters.
7. [Symbol](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol): A unique, immmutable data type.

### [Objects](https://developer.mozilla.org/en-US/docs/Glossary/Object)

Objects are a collection of properties. These properties can be of any type, including other objects. Properties are identified with *key* values.

#### Keys
Keys can either be`String` or `Symbol` values.

### [Functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Functions)

Functions are procedures a script or module can perform. They are technically a kind of object, but are generally treated separately.

### Dynamic Typing and Data Conversion
JavaScript is *dynamically typed*, meaning that a variable's type does not need to be specified when it is declared. Data types are automatically converted during script execution as needed. As such, the following is completely legal:

```
let a = 2;

a = "B";
```

### [Numbers, Strings and the `+` operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Grammar_and_types#numbers_and_the_operator)
When a `+` operator is used with a string and a number, the number is automatically typecast to a string. For example:

```
let a = 'Catch' + 22; // a = 'Catch22'
let b = '42' + 0; // b = '420'
```

This behavior does not apply to any other operator:

```
let a = '22' - 3; // 19
let b = '5' * 2; // 10
let c = 'a' / 2; // NaN
```

### Converting Strings to Numbers
There are a handful of ways to convert numerical values stored as strings into the number primitive type.

#### [parseInt(...)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseInt)
This function parses a string parameter and returns an integer of a certain [radix (base)](https://en.wikipedia.org/wiki/Radix). 

##### Parameters:

* **string:** A string to parse as an integer. Any leading whitespace is ignored.
* **radix (Optional):** An integer between `2` and `36` representing the base of the passed string. If the radix is nonzero and outside of the range `[2, 36]`, the return value of the operation will be [NaN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/NaN). If the radix is `0` or not provided, the function will interpret the radix based on the format of the string. 
  * **NOTE:** The parseInt function will not always default to decimal (base 10) when interpreting the radix. It is best practice to always include a radix value. 

##### Return Value
An integer (number) in the specified radix. 

#### [parseFloat(...)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseFloat)
This function takes a string representation of a decimal and returns its number value. 

##### Parameters
* **string**: String to parse as a float

##### Return Value
A floating point number if the string parameter is parsable, or NaN if not. 

#### [Number(...)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number#number_coercion)
Coerces the passed parameter as a number if possible. 

##### Parameter
* **value:** a value of any type. Different values/types are coerced differently. 

##### Return Value
* `number` --> Returned *as is*
* `null` --> 0
* `true` --> 1, `false` --> 0
* `undefined` --> `NaN`
* string --> Parsed as if they contain number literals

#### [Unary +](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Unary_plus)
The unary plus operator can also be used to coerce strings as numbers. It precedes the value to coerce like so:

```
let a = (+'123'); // 123
let b = (+'1') + (+'2); // 3
let c = (+null); // 0
```

Using the unary plus operator is equivalent to using the Number constructor (`Number(...)`).

## Literals
Literals are fixed values that are 'literally' specified in a script. JavaScript defines the following literals:

* [Array Literals](#array-literals)
* [Boolean Literals](#boolean-literals)
* [Numeric Literals](#numeric-literals)
* [Object Literals](#object-literals)
* [RegExp Literals](#regexp-literals)
* [String Literals](#string-literals)

### Array Literals
A representation of an [array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array#description). More specifically, an array literal is a list of expressions separated by commas and enclosed in square brackets where each expression represents an array element:

```
// specifies an array of size 3 with elements 1, 2, and 3
[1, 2, 3]; 
```

A new array is created every time the array literal is evaluated in the code. For example, an array literal at the top-level, global scope will create an array once when the script/module is loaded, whereas a literal inside a function will create an array every time the function is called.

#### Extra Commas
Placing two commas in a row, or prepending a comma at the front of an array literal creates an *empty slot* in the array. 

This empty slot is slightly different from `undefined` - when traversing the array, the empty value will be skipped. However, accessing the empty element will return an `undefined`. The size of the array will include the empty elements. 

```
// creates an array of size 4 with two empty items
const withEmpties = [, 'a', , 'b']; 
```

A comma appended to the end of an array literal will be ignored.

##### Trailing Commas and Git Diffs
Trailing commas tend to be added after the last element of a multiline array literal to clean up Git Diffs. In this way, the previous line does not need to be changed by adding a comma if a new line is added:

```
const arr = [
  'a',
  'b',
  'c', // <-- no comma needs to be added for new element
]
```

##### Best Practice
It is best practice to either explicitly state undefined elements, or at least add comments specifying empty elements for clarity's sake:

```
// specifying undefined
const withUndefined = ['a', undefined, 'b'];

// with comments
const with Comments = ['a', /* empty */, 'b'];
```

### Boolean Literals
A boolean literal can be one of two values: `true` or `false`.

NOTE: Do not confuse the `true` and `false` values of the boolean primitive with those of the [Boolean](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean) wrapper class. They are different.

### Numeric Literals
Numeric literals include base 10 floating point literals and integer literals in other bases. 

The language specification requires that numeric literals be unsigned, but something like `-101.1` is considered valid because the `-` is treated as a unary operator applied to `101.1`.

#### Integer Literals
Integer and [BigInt](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/BigInt) literals can be written in the following bases:
* 10 (Decimal)
* 2 (Binary)
* 16 (Hexadecimal)
* 8 (Octal)

The following rules determine which base and format an integer literal is in:

* A sequence of digits *without a leading zero* is a decimal integer literal.
* A leading `0`, `0o`, or `0O` indicates an octal integer literal, which can include only the digits 0 - 7.
* A leading `0x` or `0X` indicates a hexadecimal integer literal. Hexadecimal numbers can have the digits 0 - 9 plus the letters A - F (case insensitive).
* A leading `0b` or `0B` indicates a binary integer literal, which can only have the digits 0 and 1. 

#### Floating Point Literals
A floating point literal can be composed of the following components:
* Unsigned decimal integer
* Decimal point (`.`)
* Decimal integer representing a fraction
* An exponent (`e` or `E` followed by an optionally signed decimal integer)

The format of a floating point literal is specified by the following RegExp:

```
[digits].[digits][(E|e)[(+|-)]digits]
```

Some examples of floating point literals:

```
121.2
-54.2
1e1 // 10
2.0E-5
51E+2
```


### Object Literals
Stub 

### RegExp Literals
Stub 

### String Literals
Stub