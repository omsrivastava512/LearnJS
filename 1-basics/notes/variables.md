# **Javascript Fundamentals**

## **Variables**

### **1. Dynamically Typed**

* Variables in Javascript are not of fixed type
* Type is determined at runtime and can change
* Example:

```javascript
let x = 5;        // x is a number
x = "hello";      // now x is a string
x = true;         // now x is a boolean
```

### **2. Variable Declaration**

* No type declaration needed
* Variables are automatically initialized to `undefined` unless `const`

```javascript
var x;           // undefined
let y;           // undefined  
const z = 10;    // Must initialize const
```

### **3. Memory Management**

* Automatic garbage collection
* All variables are references (except primitives)
* No manual memory management

```javascript
let x = 5;           // Primitive value
let obj = {a: 1};    // Object reference
// Garbage collector handles everything
```

### **4. Scope Rules**

* Function scope (`var`)
* Block scope (`let`, `const`)
* Global scope

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

* `var` declarations are hoisted
* `let` and `const` have temporal dead zone

```javascript
console.log(x); // undefined (not error, due to hoisting)
var x = 5;

console.log(y); // ReferenceError: Cannot access 'y' before initialization
let y = 10;
```

## **6. Mutability**

* `const` prevents reassignment
* Object/array contents can still be modified

```javascript
const x = 5;               // Cannot reassign x
const obj = {a: 1};
obj.a = 2;                 // OK * modifying property
obj.b = 3;                 // OK * adding property
```

## **7. Type Coercion**

* Automatic type coercion
* Can lead to unexpected results

```javascript
let x = 5;
let y = "10";
console.log(x + y);    // "510" (string concatenation)
console.log(x * y);    // -5 (numeric subtraction)
console.log(x == y);   // false
console.log(10 == y);   // true
```

## **Variables (Deep Dive)**

### **1. Javascript's Value Storage Strategy**

**Small Integers (SMI * Small Integer)**:

```javascript
let x = 42;  // Likely stored as immediate value (no heap allocation)
let y = 1000000000; // May require heap allocation as boxed number
```

**String Internment**:

```javascript
let str1 = "hello";  // Stored in string pool
let str2 = "hello";  // Points to same memory location
```

**Object Storage**:

```javascript
let obj = {
    name: "John",    // Property stored separately
    age: 30,         // May be stored as SMI
    address: {...}   // Nested object, separate heap allocation
};
```

**Hidden Classes/Maps (V8 Engine)**:

* Objects with same structure share "hidden classes"
* Properties stored in separate backing store
* Optimizes property access through inline caching

### **2. Memory Regions in JavaScript Engines**

**V8 Engine Example:**

```md
┌─────────────────┐
│   Stack         │ ← Function calls, local var references
├─────────────────┤
│   Young Gen     │ ← New objects (Eden, Survivor spaces)
│   (Heap)        │
├─────────────────┤
│   Old Gen       │ ← Long-lived objects
│   (Heap)        │
├─────────────────┤
│   Large Object  │ ← Objects > 512KB
│   Space         │
├─────────────────┤
│   Code Space    │ ← Compiled JavaScript code
└─────────────────┘
```

**Primitive Values**:

```javascript
let x = 42;        // SMI: stored as tagged immediate value
let y = 3.14;      // Heap: double object with type tag
```

**Strings**:

```javascript
let str = "hello";     // Heap-allocated string object
// Includes: type info, length, character data, possibly rope structure
```

**Objects/Arrays**:

```javascript
let p = {age: 25, name: "John"};
// Heap: [hidden class pointer|properties backing store]
// Properties may be stored inline or in separate array
```

### **3. Memory Layout Examples**

**JavaScript Object Memory Layout (V8)**:

```javascript
let obj = {x: 1, y: 2};
```

**Memory Structure:**:

```md
Stack:
[obj] → heap_address

Heap:
heap_address: [Map pointer|Properties|Elements]
                    ↓           ↓         ↓
            Hidden Class    [x:1, y:2]   []
```

**Array Storage**:

```javascript
let arr = [1, 2, "hello", {a: 1}];
```

**V8 Storage:**:

* **Elements backing store**: `[SMI:1, SMI:2, string_ref, object_ref]`
* **Different element types**: May cause "holey arrays" optimization degrade

### **4. Performance Implications**

**Memory Overhead:**

```javascript
// JavaScript object overhead example
let num = 42;           // ~8 bytes (tagged value or boxed)
let str = "a";         // ~24+ bytes (object header + data)
let obj = {};          // ~32+ bytes (object + hidden class)

// Compare to C:
int num = 42;          // 4 bytes exactly
char str = 'a';        // 1 byte exactly
```
