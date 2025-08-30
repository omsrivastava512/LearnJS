# **Javascript Basics** (AI Assisted)

## Table of Contents

- [Part 1](#part-1)
  - [Syntax & Grammar](#syntax--grammar)
  - [Variables](#variables)
    - [`var` (The Old Way)](#var-the-old-way)
    - [`let` (The Modern Way)](#let-the-modern-way)
    - [`const` (For Constants)](#const-for-constants)
  - [Data Types](#data-types)
    - [Primitive Types](#primitive-types)
    - [Object Type](#object-type)
- [Part 2](#part-2)
  - [Operators](#operators)
    - [Arithmetic Operators](#arithmetic-operators)
    - [Assignment Operators](#assignment-operators)
    - [Comparison Operators](#comparison-operators)
    - [Logical Operators](#logical-operators)
    - [Bitwise Operators](#bitwise-operators)
  - [Control Flow](#control-flow)
    - [`if` / `else if` / `else`](#if--else-if--else)
    - [`switch`](#switch)
    - [Ternary Operator](#ternary-operator)
  - [Loops](#loops)
    - [`for` Loop](#for-loop)
    - [`while` Loop](#while-loop)
    - [`do...while` Loop](#dowhile-loop)
    - [`for...of` Loop (Modern)](#forof-loop-modern)
    - [`for...in` Loop](#forin-loop)

(AI Assisted/Generated)

## Part 1

### Syntax & Grammar

This is the set of rules that dictates how a valid JavaScript program is written.  The core components are statements and expressions.

- **Statements**: A statement is a complete instruction that performs an action.  For example, declaring a variable or logging to the console are statements. Each statement typically ends with a semicolon or next line. This means that statements can either be seperated by next line or by a semicolon or both.

    ```javascript
    let message = "Hello, world!" // A declaration statement (next line seperated)
    let a = 5; let b = 8;          // Multiple declarations (comma serpated)
    console.log(message);          // A call statement (both)
    ```

- **Expressions**: An expression is any piece of code that resolves to a single value. It's like a phrase that represents a noun. `2 + 2` is an expression that resolves to the value `4`. The string `"Hello"` is also an expression.

    ```javascript
    5 * 10       // Resolves to the value 50
    "Hello" + " " + "World" // Resolves to "Hello World"
    isReady      // Resolves to the value stored in the isReady variable
    a = 5        // resolveds to the value on the right (5)
    b = 5 * a    // resolved to 25
    ```
  
  - Expressions can be comma seperated and one or more expressions can form a statement.
  
  ```javascript
    let a = 5, b = 7, c = a + b;    // comma seperated expressions
    let result = (x = 3, y = 4, x + y); // result is 7  
    // The comma operator evaluates each expression from left to right and returns the last one.
    sum(1, 2, 3); // ❌Expressions ✅Arguments 

    // comma seperated expressions in loops
    for (let i = 0, j = 10; i < j; i++, j--) {  
     console.log(i, j);
    }   

  ```

- **Semicolons (;)**: Semicolons are used to **separate statements**. While JavaScript has a feature called **Automatic Semicolon Insertion (ASI)** that often lets you omit them, it's a best practice to **always use them**. Relying on ASI can lead to subtle and hard-to-find bugs.

-----

### Variables

Variables are named containers for storing data values. In JavaScript, you declare variables using one of three keywords: `var`, `let`, or `const`. The key difference between them is their **scope** (where they are accessible) and whether they can be reassigned.

#### `var` (The Old Way)

The `var` keyword has been around since the beginning of JavaScript. It is **function-scoped**.

- **Scope**: A variable declared with `var` is accessible anywhere within the **function** it's declared in, regardless of block (`{...}`). For example,

```javascript
for (var i = 0; i < 5; i++);
console.log(i);     // i will be accessible
```

- **Reassignment & Redeclaration**: You can update its value and even re-declare the same variable without an error.
- **Hoisting**: `var` **declarations** are "hoisted" to the top of their scope, meaning they are processed before any code is executed. They are initialized with `undefined`.

<!-- end list -->

```javascript
function varTest() {
  var x = 1;
  if (true) {
    var x = 2; // Same variable!
    console.log(x); // Outputs 2
  }
  console.log(x); // Outputs 2, because x is function-scoped
}
```

#### `let` (The Modern Way)

Introduced in ES6 (2015), `let` is the preferred way to declare variables whose value might change. It is **block-scoped**.

- **Scope**: A variable declared with `let` is only accessible within the **block** (`{...}`) where it's defined. This is more predictable and less error-prone.
- **Reassignment & Redeclaration**: You can update its value, but you **cannot** re-declare it in the same scope.
- **Hoisting**: `let` variables are also hoisted but are not initialized. Accessing them before declaration results in a `ReferenceError` (this is called the "Temporal Dead Zone").

<!-- end list -->

```javascript
function letTest() {
  let y = 1;
  if (true) {
    let y = 2; // Different variable, scoped to this block
    console.log(y); // Outputs 2
  }
  console.log(y); // Outputs 1, because the outer y is unaffected
}
```

#### `const` (For Constants)

Also introduced in ES6, `const` is for declaring variables that should not be reassigned. It is also **block-scoped**.

- **Scope**: Just like `let`, `const` is **block-scoped**.
- **Reassignment & Redeclaration**: You **must** assign a value at declaration, and you **cannot** update the variable's assignment or re-declare it.
- **Important Caveat**: For objects and arrays declared with `const`, the variable itself cannot be reassigned, but the **properties of the object or the elements of the array can be changed**.

<!-- end list -->

```javascript
const PI = 3.14159;
// PI = 3; // This would throw a TypeError

const person = { name: "Alice" };
person.name = "Bob"; // This is allowed!
// person = { name: "Charlie" }; // This would throw a TypeError
```

**Rule of thumb**: Always use `const` by default. If only you know you'll need to reassign the variable, use `let`. Avoid using `var` in modern JavaScript.

-----

### Data Types

JavaScript is a **dynamically typed** language, which means you don't need to specify the type of data a variable will hold. The engine figures it out at runtime based on the value it holds. Data types are categorized into two main groups: **Primitive Types** and the **Object Type**.

#### Primitive Types

These are the most basic data types. They are **immutable**, meaning their value cannot be changed once created.

1. **String**: Used for textual data. You can use single quotes (`'...'`), double quotes (`"..."`), or backticks (`` `...` ``). Backticks allow for multi-line strings and embedded expressions (template literals).

    ```javascript
    let name = "Gemini";
    let greeting = `Hello, ${name}!`; // "Hello, Gemini!"
    ```

2. **Number**: Represents both integer and floating-point numbers. It also includes special values like `Infinity`, `-Infinity`, and `NaN` (Not-a-Number).

    ```javascript
    let age = 30;
    let price = 19.99;
    let result = 0 / 0; // NaN
    ```

3. **BigInt**: For whole numbers larger than the `Number` type can safely represent ($2^{53}-1$). Created by appending `n` to the end of an integer.

    ```javascript
    const veryLargeNumber = 9007199254740991n;
    const veryLargeNumber = new BigInt('123445')
    ```

4. **Boolean**: Represents a logical entity and can have two values: `true` and `false`.

    ```javascript
    let isLoggedIn = true;
    let isComplete = false;
    ```

5. **Undefined**: A variable that has been declared but has **not yet been assigned** a value has the type and value `undefined`.

    ```javascript
    let job;
    console.log(job); // Outputs: undefined
    ```

6. **Null**: Represents the intentional absence of any object value. It's a special value that you can assign to a variable to signify **"no value"**.

    ```javascript
    let currentUser = null; // No user is logged in
    ```

7. **Symbol**: A unique and immutable primitive value that may be used as the key of an Object property. It's mainly used to create unique identifiers.

    ```javascript
    const id = Symbol('id');
    ```

#### Object Type

The **Object** type is a collection of key-value pairs. It's the only complex data type and is used to store collections of data and more complex entities. Arrays, functions, and dates are all special types of objects in JavaScript.

```javascript
// A standard object
let car = {
  make: "Honda",
  model: "Civic",
  year: 2025
};

// An array (a special type of object for ordered lists)
let colors = ["red", "green", "blue"];
```

## Part 2

### Operators

Operators are special symbols used to perform operations on values and variables (operands).

#### Arithmetic Operators

These are for performing standard mathematical calculations.

- `+` (Addition)
- `-` (Subtraction)
- `*` (Multiplication)
- `/` (Division)
- `%` (Modulus/Remainder)
- `**` (Exponentiation - ES6)
- `++` (Increment)
- `--` (Decrement)

<!-- end list -->

```javascript
let x = 10;
let y = 4;
console.log(x + y); // 14
console.log(x % y); // 2 (10 divided by 4 is 2 with a remainder of 2)
console.log(2 ** 3); // 8 (2 to the power of 3)
x++; // x is now 11
```

#### Assignment Operators

These are used to assign values to variables.

- `=` (Assignment)
- `+=` (Add and assign)
- `-=` (Subtract and assign)
- `*=` (Multiply and assign)
- `/=` (Divide and assign)

<!-- end list -->

```javascript
let score = 100;
score += 50; // score is now 150 (same as score = score + 50)
score -= 25; // score is now 125
```

#### Comparison Operators

These are used to compare two values, resulting in a boolean (`true` or `false`).

- `==` (Loose Equality): Compares values after **type coercion**. It tries to convert the values to a common type before comparing. **Avoid this.**
- `===` (Strict Equality): Compares both value and type. **Always use this.**
- `!=` (Loose Inequality)
- `!==` (Strict Inequality)
- `>` (Greater than), `<` (Less than)
- `>=` (Greater than or equal to), `<=` (Less than or equal to)

The distinction between `==` and `===` is critical:

```javascript
console.log(7 == "7");   // true (string "7" is coerced to number 7)
console.log(7 === "7");  // false (number is not the same type as string)

console.log(0 == false); // true (false is coerced to number 0)
console.log(0 === false);// false (number is not the same type as boolean)
```

**Rule of thumb**: Always use strict equality (`===` and `!==`) to prevent unexpected bugs from type coercion.

#### Logical Operators

These are used to combine multiple boolean expressions.

- `&&` (Logical AND): Returns `true` only if **both** operands are true.
- `||` (Logical OR): Returns `true` if **at least one** operand is true.
- `!` (Logical NOT): Inverts a boolean value (`true` becomes `false`, `false` becomes `true`).

<!-- end list -->

```javascript
let isAdult = true;
let hasTicket = false;

console.log(isAdult && hasTicket); // false
console.log(isAdult || hasTicket); // true
console.log(!isAdult);             // false
```

#### Bitwise Operators

These treat numbers as a sequence of 32 bits (zeros and ones) and perform operations on them. They are less common in everyday web development but are used in performance-critical or low-level operations. Examples include `&` (AND), `|` (OR), and `^` (XOR).

-----


### Type Conversion (Explicit) 👉

This is when you, the developer, deliberately change a value's type. It's intentional, predictable, and generally considered good practice.

However, one can convert to only one of the three types in JS.

**To a String:**

- `String(value)`: The safest way. Works on `null` and `undefined`.
- `value.toString()`: Works for almost everything but will throw an error on `null` or `undefined`.

  <!-- end list -->

  ```javascript
  String(123); // "123"
  String(null); // "null"
  (123).toString(); // "123"
  // null.toString(); // Throws a TypeError
  ```

**To a Number:**

- `Number(value)`: The standard way. Converts the whole string. Returns `NaN` (Not a Number) if the string can't be fully converted. However, NaN is not exactly "not a number" but a value to represent **invalid number**. Try `typeof NaN`.
- `parseInt(value, radix)`: Parses until it hits a non-numeric character or a fractional value. The second argument (`radix`, usually 10) is used to interpret the base of the numeral system the string hold numbers in.
- `parseFloat(value)`: Similar to `parseInt` but includes decimal points.
- **Unary Plus Operator (`+`)**: A common shortcut to convert a value to a number.
    <!-- end list -->

  ```javascript
  Number("123.45"); // 123.45
  Number("123a");   // NaN
  parseInt("123a");  // 123
  parseInt("55",8);  // 45 -> 55 in octal parses to 45 in decimal
  +"123.45";        // 123.45 (shorthand for Number)
  ```

**To a Boolean:**

- `Boolean(value)`: The standard constructor.
- **Double Negation (`!!`)**: A popular and concise shortcut to force a value to its boolean equivalent.

    <!-- end list -->

    ```javascript
    Boolean("hello"); // true
    Boolean(0);       // false
    !!"hello";        // true (shorthand)
    !!0;              // false
    ```

-----

### Control Flow

Control flow statements allow you to control the execution path of your code based on certain conditions.

#### `if` / `else if` / `else`

This is the most common way to make a decision. The code block of the first `true` condition is executed.

```javascript
let temperature = 25;

if (temperature > 30) {
  console.log("It's hot outside!");
} else if (temperature > 20) {
  console.log("It's a pleasant day.");
} else {
  console.log("It's a bit chilly.");
}
// Outputs: "It's a pleasant day."
```

#### `switch`

A `switch` statement is a cleaner alternative to a long `if/else if/else` chain when you're comparing a single value against multiple possibilities.

- `case`: Defines a block of code to be executed if the expression matches the case.
- `break`: Stops the execution from "falling through" to the next case.
- `default`: An optional clause that runs if no case matches.

<!-- end list -->

```javascript
let day = new Date().getDay(); // Returns a number (0=Sunday, 1=Monday, etc.)

switch (day) {
  case 0:
    console.log("It's Sunday!");
    break;
  case 6:
    console.log("It's Saturday!");
    break;
  default:
    console.log("It's a weekday.");
}
```

#### Ternary Operator

The ternary (or conditional) operator is a compact, one-line `if/else` statement. Its syntax is `condition ? expressionIfTrue : expressionIfFalse`.

```javascript
let age = 20;
let canDrink = (age >= 18) ? "Yes" : "No";

console.log(canDrink); // Outputs: "Yes"
```

-----

### Loops

Loops allow you to execute a block of code repeatedly.

#### `for` Loop

The classic loop, used when you know how many times you want to iterate. It consists of three parts: initialization, condition, and increment. Best when you need tight control over the iteration. This includes iterating a specific number of times, iterating backward, or having complex logic for the increment step (e.g., i += 2). It gives you direct access to the index i.

```javascript
// Prints numbers 0 through 4
for (let i = 0; i < 5; i++) {
  console.log(i);
}
```

#### `while` Loop

A `while` loop executes as long as a specified condition is true. The condition is checked **before** each iteration.

```javascript
let count = 0;
while (count < 3) {
  console.log("Counting...");
  count++;
}
// Prints "Counting..." three times
```

#### `do...while` Loop

This is similar to a `while` loop, but the code block is executed at least **once** because the condition is checked **after** the iteration.

```javascript
let password;
do {
  // This prompt will always appear at least once
  password = prompt("Enter the password:");
} while (password !== "secret");
```

> `while` and `do-while` loop are perfect for when you don't know how many iterations you need. A classic example is fetching data from a paginated API until there are no more pages.
However, in these you are responsible for updating the variable that controls the condition. Forgetting to do so is the most common cause of infinite loops.

#### `for...of` Loop (Modern)

Used to iterate over **iterable objects only** (like Arrays, Strings, Maps, etc.). It gives you the **value** of each element directly. This is the preferred way to loop over arrays. Using this on plain (non-iterable) object will throw `TypeError`.

```javascript
const team = ["Alice", "Bob", "Charlie"];

for (const member of team) {
  console.log(member); // Alice, Bob, Charlie
}
```

> In case you need both the value and the index, you can use `Array.prototype.entries()` with it. It returns an `Iterator` object with each element has a value property holding an array such as `[index, value]`.

```javascript
console.log(team.entries().next()) // { "value": [0,1], "done": false } 
console.log(team.entries().toArray()) // [ [0,"Alice"], [1,"Bob"], ["2", "Charlie"] ]

// Getting index and value
for (const [index, member] of team.entries()) {
  console.log(`${index}: ${member}`); // 0: Alice, 1: Bob, 2: Charlie
}
```

#### `for...in` Loop

Used to iterate over the **enumerable(listable) properties (keys)** of an **object**. Do not use this for arrays. The iteration order is not guaranteed and it can include inherited properties from the prototype chain. Therefore, always check check `if (obj.hasOwnProperty(key))` to avoid iterating over inherited properties.

```javascript
const person = {
  name: "Alice",
  age: 30,
  city: "New York"
};

for (const key in person) {
    if (person.hasOwnProperty(key)) console.log(`${key}: ${person[key]}`);
}
// Outputs:
// "name: Alice"
// "age: 30"
// "city: New York"
```

> All the properties of an object are enumerable **by default**. You can explicitly make a property non-enumerable using Object.defineProperty():

```javascript
Object.defineProperty(obj, "secret", {
  value: "hidden",
  enumerable: false
});
// obj.secret exists but won’t show up in for...in or Object.keys().
```

|Concept | Applies To | Meaning|
|--------|-----------|------|
| Enumerable | Object properties | Can be listed using `for...in`, `Object.keys()`, etc.|
| Iterable | Whole objects | Can be looped over using for...of (must have a [Symbol.iterator])|

**Iterators** often use the prototype `forEach()` method when you want to execute a function for each item in an array and don't need to break out of the loop.  The return value of `forEach()` is always `undefined`. And there is no way to stop a forEach() loop early. It will always run for every element in the array.

-----
