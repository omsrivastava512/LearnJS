# JavaScript Basics: Clean Code Patterns & Best Practices

## Strict Mode

Introducted in ES5. Helps avoid common coding mistakes and "unsafe" actions by imposing restrictions.

### How to enable?

- **Globally**

    ```javascript
    'use stript';  //  at the top of the script 
    ```

- **Locally**

    ```javascript
    function fn(){
    'use script' // at the top of the scope
    }
    ```

### Benefits

- Disallows **undeclared** variables

    ```javascript
    'use strict';
    x = 10; // Error: x is not declared
    ```

- Blocks duplicate parameter names

    ```javascript
    function sum(a, a) { } // Error in strict mode /

    // This is by default erroneous in arrow functions
    // Note that they both are treated as one variable only without strict
    // Thus, there is no naming conflict here 
    // The latter argument will be the final assignment
    // E.g. sum(5,6) -> 12 since `a` is reassigned with 6
    ```

- Disallows using **future reserved words** to be used as names

    ```javascript
    `use strict`
    let private ;
    let private ;
    ```

## Functions

### Function Declarations vs. Expressions (Hoisting)

- *Function Declarations*

    ```javascript
    function sum1 (a,b) {return a+b}
    ```

- **Function Expressions**

    ```javascript
    const sum2= function (a,b) {return a+b}
    ```

> The key difference between function declarations and function expression is that a function declaration is hoisted at the top of the script by the JavaScript compiler and thus, we can call a function declaration before  it is declared.

Prefer function expressions assigned to `const`. This prevents accidental redeclaration and calling it before it's defined. It enforces a logical, top-down code flow.

```javascript
// Hoisted: Can be called before it's defined (can lead to messy code)
sayHello();
function sayHello() {
    console.log("Hello!");
}

// Not Hoisted: Throws a ReferenceError if called before definition (predictable)
// sayGoodbye(); // This would crash
const sayGoodbye = () => {
    console.log("Goodbye!");
};
sayGoodbye();
```
