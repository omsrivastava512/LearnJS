# **Javascript Fundamentals**

## **Variables**

### **1. Dynamically Typed**

- Variables in Javascript are not of fixed type
- Type is determined at runtime and can change
- Example:

```javascript
let x = 5;        // x is a number
x = "hello";      // now x is a string
x = true;         // now x is a boolean
```

### **2. Variable Declaration**

- No type declaration needed
- Variables are automatically initialized to `undefined` unless `const`

```javascript
var x;           // undefined
let y;           // undefined  
const z = 10;    // Must initialize const
```

### **3. Memory Management**

- Automatic garbage collection
- All variables are references (except primitives)
- No manual memory management

```javascript
let x = 5;           // Primitive value
let obj = {a: 1};    // Object reference
// Garbage collector handles everything
```

### **4. Scope Rules**

- Function scope (`var`)
- Block scope (`let`, `const`)
- Global scope

```javascript
var globalVar = 1;       // Global scope
function example() {
    var funcVar = 2;     // Function scope
    if (true) {
        var varInBlock = 3;   // Still function scope!
        let letInBlock = 4;   // Block scope
        const constInBlock = 5; // Block scope
    }
    // varInBlock accessible here, but not letInBlock or constInBlock
}
```

### **5. Hoisting Behavior**

- `var` declarations are hoisted
- `let` and `const` have temporal dead zone

```javascript
console.log(x); // undefined (not error, due to hoisting)
var x = 5;

console.log(y); // ReferenceError: Cannot access 'y' before initialization
let y = 10;
```

## **6. Mutability**

- `const` prevents reassignment
- Object/array contents can still be modified

```javascript
const x = 5;               // Cannot reassign x
const obj = {a: 1};
obj.a = 2;                 // OK - modifying property
obj.b = 3;                 // OK - adding property
```

## **7. Type Coercion**

- Automatic type coercion
- Can lead to unexpected results

```javascript
let x = 5;
let y = "10";
console.log(x + y);    // "510" (string concatenation)
console.log(x - y);    // -5 (numeric subtraction)
console.log(x == y);   // false
console.log(10 == y);   // true
```
