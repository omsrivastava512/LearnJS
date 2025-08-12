# **Javascript Core**

## **Brief**

JavaScript stores variables in two primary memory locations: the **Stack** and the **Heap**. The type of data determines where it's stored.

* **Primitive Types are stored on the Stack.** These include `Number`, `String`, `Boolean`, `null`, `undefined`, `Symbol`, and `BigInt`. They are stored **by value**. This means the variable holds the actual data directly. The Stack is a fast, fixed-size, last-in-first-out (LIFO) data structure used for static memory allocation.

* **Reference Types are stored on the Heap.** These include `Object`, `Array`, and `Function`. They are stored **by reference**. This means the variable on the Stack holds a *pointer* (a memory address) to the actual object, which resides in the Heap. The Heap is a larger, less organized region of memory for dynamic memory allocation.

-----

### How Different Types Are Stored and Called

* **Primitives (Number, String, etc.):**
  * **Storage:** When you declare `let score = 100;`, the value `100` is placed directly onto the stack at the memory location associated with `score`.
  * **Calling:** When you use `score`, the engine goes directly to that spot on the stack and retrieves the value `100`.

* **Objects & Arrays (Reference Types):**
  * **Storage:** When you declare `let user = {name: "Alex"};`, two things happen:
        1.  The object `{name: "Alex"}` is created in the Heap.
        2.  A pointer (e.g., memory address `0x1A2B`) is placed on the Stack at the location for the `user` variable. This pointer "points" to the object in the Heap.
  * **Calling:** When you access `user.name`, the engine first goes to the `user` variable on the Stack, gets the pointer (`0x1A2B`), then follows that pointer to the object on the Heap to find the `name` property.

* **Functions:**
  * **Storage:** Functions are a special type of object and are also stored by reference. The entire function body is placed on the Heap, and a pointer to it is stored in the variable on the Stack.
  * **Calling:** When you invoke `myFunction()`, the engine follows the pointer from the Stack to the Heap to find the function's code and then executes it, creating a new execution context on the call stack.

* **Other Primitive Types:**
  * `Symbol`: Symbols are simple and their entire purpose is to be unique identifiers. They are true primitives and are stored by value on the stack. Each `Symbol()` call creates a new, unique value.

  ```javascript 
    let sym1 = Symbol('id');
    let sym2 = Symbol('id');
    console.log(sym1 === sym2); // false - Proves they are unique values.
  ```

  * `BigInt`: BigInt is considered a primitive type, but because its size can be enormous, far exceeding the fixed size of a stack slot, larger than Number.MAX_SAFE_INTEGER (which is 2⁵³ - 1), its data is stored on the heap. The variable on the stack holds a reference to the BigInt data on the heap. ``` let bigN = 12345n, bigM = bigInt("122434"); ```

    * However, just like strings, BigInts are immutable. You cannot change a BigInt in place. Any mathematical operation on a BigInt creates a new BigInt object on the heap, and the variable's reference is updated. This allows it to maintain primitive-like "by value" behavior even though it's stored "by reference."

-----

### Time of Interpretation (Hoisting)

Hoisting is JavaScript's behavior of moving declarations to the top of their scope before code execution. This happens during the "creation phase" of the execution context.

* **Functions:** Entire function declarations (`function name() {...}`) are hoisted. This means you can call a function *before* it appears in your code.

    ```javascript
    sayHello(); // Works! Outputs: "Hello there"

    function sayHello() {
        console.log("Hello there");
    }
    ```

* **Variables (`var`, `let`, `const`):**

  * `var`: Declarations are hoisted and initialized with a default value of `undefined`.
        ```javascript
        console.log(myVar); // Outputs: undefined (No error)
        var myVar = 10;
        ```
  * `let` and `const`: Declarations are hoisted but **not initialized**. They are in a "Temporal Dead Zone" (TDZ) from the start of the scope until the line where they are declared. Accessing them in the TDZ throws a `ReferenceError`.
        ```javascript
        console.log(myLet); // Throws ReferenceError: Cannot access 'myLet' before initialization
        let myLet = 20;
        ```

-----

### What Happens When a Variable's Type Changes?

This is a key feature of JavaScript: it is a **dynamically-typed language**. The variable itself does not have a type; the **value it holds** does. The interpreter figures out the type at runtime.

Let's trace an example: `let data = "hello";` and then `data = 123;`

1. **`let data = "hello";`**

      * The JavaScript engine allocates space on the **stack** for a variable named `data`.
      * It sees the value is `"hello"`, a primitive string.
      * It stores the value `"hello"` on the heap's string pool. However, because strings are immutable (they cannot be changed), JavaScript can ensure they behave just like true primitives (pass-by-value)

2. **`data = 123;`**

      * The engine encounters the reassignment.
      * It simply **overwrites** the existing value at the memory location for `data` on the stack.
      * The old value `"hello"` is now gone (and will be garbage collected). The new primitive number `123` now occupies that space.

-----

### String Interning

When the engine encounters a string literal (e.g., "hello"), it checks an internal, table-like structure (often on the Heap) to see if that exact string already exists.

* If it exists, the variable on the stack gets a pointer to that existing string in the pool.
* If it doesn't exist, the string is created in the pool, and the variable gets a pointer to it.

    ```javascript
    let a = "reusable string";
    let b = "reusable string";

    // a and b both point to the *exact same* memory location in the string pool.
    // This is an engine optimization to save memory.
    ```

Objects and Arrays are designed to be mutable (changeable). Their properties and elements can be added, removed, or modified at any time. If JavaScript "pooled" arrays, it would lead to chaos.

-----

### The Call Stack

The Call Stack's primary job is to manage **function execution**. It keeps track of which function is currently running and where to go next when that function finishes. It operates on a **Last-In, First-Out (LIFO)** principle.

* **What it Stores:**
  * **Execution Contexts:** When a function is called, a "stack frame" is created and pushed onto the top of the stack. This frame contains the function's arguments, local variables, and the return address.
  * **Primitive Types:** Variables holding **primitive values** (`Number`, `Boolean`, `null`, `undefined`, `Symbol`) are stored directly in the stack frame. This makes them extremely fast to access.
  * **References:** For complex types, the stack stores a **pointer** (a memory address) to the object's actual location in the Heap.

* **Key Characteristics:**
  * **Fast:** Accessing memory on the stack is very fast because it's a highly organized, linear structure.
  * **Fixed Size:** The stack has a limited size. If you have too many nested function calls (like in infinite recursion), you'll run out of space, which results in a **"Stack Overflow"** error.

### The Heap

The Heap is a large, unstructured region of memory used for **dynamic memory allocation**. It's where all the complex data structures in your program live.

* **What it Stores:**
  * **Reference Types:** All **objects** (`Object`, `Array`, `Function`, etc.) are stored in the Heap. Their size isn't known at compile time and can change, so they need a flexible storage space.

* **How it Works with the Stack:**
    When you declare `let user = {name: "Alex"};`:
    1. The object `{name: "Alex"}` is created and stored somewhere in the vast Heap.
    2. The `user` variable is placed on the **Stack**, but instead of holding the whole object, it holds a **reference** (e.g., memory address `0x1A2B`) that points to the object's location in the Heap.

* **Key Characteristics:**
  * **Slower Access:** It's slower than the stack because it requires following a reference to find the data.
  * **Large and Flexible:** It's much larger than the stack and can grow as your application needs more memory.
  * **Garbage Collection:** The JavaScript engine has a process called a "Garbage Collector" that periodically scans the Heap. It finds and removes objects that are no longer referenced by any variable on the Stack, freeing up memory and preventing leaks.

### The Code Cache

The Code Cache is a memory area dedicated to **storing compiled machine code** for performance. JavaScript is an interpreted language, but modern engines use a technique called **Just-In-Time (JIT) Compilation** to make it run much faster.

* **What it Stores:**
  * **Optimized Machine Code:** The raw, low-level code that your computer's processor can execute directly.

* **How it Works (JIT Compilation):**
  1. **Interpretation:** Initially, the engine's interpreter reads your JavaScript and executes it line by line.
  2. **Monitoring:** At the same time, a profiler watches your code for parts that are executed frequently (so-called "hot" functions or loops).
  3. **Compilation:** These "hot" sections are passed to an optimizing compiler. It converts them into highly efficient machine code.
  4. **Caching:** This machine code is then stored in the **Code Cache**.
  5. **Execution:** The next time that "hot" function is called, the engine skips the slow interpretation step and executes the super-fast machine code directly from the cache.

* **Key Characteristics:**
  * **Speed:** It's the reason modern JavaScript is incredibly fast. It combines the flexibility of an interpreted language with the raw power of a compiled one.
  * **Browser Feature:** You'll see this in action when you reload a webpage. The browser can cache the compiled code, making subsequent loads of the same script much faster.

```md
┌─────────────────────────────────────┐
│         JavaScript Engine           │
├─────────────────────────────────────┤
│  Parser → AST → Bytecode → Machine  │
│  Code (JIT Compilation)             │
├─────────────────────────────────────┤
│         Memory Management           │
│  ┌─────────┬─────────┬─────────────┐│
│  │  Stack  │  Heap   │ Code Cache  ││
│  └─────────┴─────────┴─────────────┘│
└─────────────────────────────────────┘
```

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
