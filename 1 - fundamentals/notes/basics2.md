# JavaScript Basics: Clean Code Patterns & Best Practices

## Strict Mode

Introducing `strict mode`! ES5 now lets you put a leash on your code. Just slap it at the top of your file or function, and JS gets serious—no more sloppy mistakes or unsafe actions.

### How to enable?

- **Globally**

    ```javascript
    'use stript';  //  at the top of the script
    ```

- **Locally**

    ```javascript
    function fn(){
        'use script' // at the top of the scope
        ...
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

- **Function Declarations**

    ```javascript
    function sum1 (a,b) {return a+b}
    ```

- **Function Expressions**

    ```javascript
    const sum2= function (a,b) {return a+b}
    ```

The key difference between function declarations and function expression is that a function declaration is hoisted at the top of the script by the JavaScript compiler and thus, we can call a function declaration before  it is declared.

> Prefer function expressions assigned to `const` to keep things locked down. No **accidental redeclaration**. No calling the shots too early— this isn't your bed. Let your code flow top-down like a good story, not how the Avengers messed up time travel.

```javascript
// Hoisted: Can be called before it's born but don't be this guy.
sayHello();
function sayHello() {
    console.log("Hello!");
}

// Not Hoisted: Throws a ReferenceError if called before definition (predictable)
// sayGoodbye(); // This would crash
const sayGoodbye = () => {
    console.log("Goodbye!");
};
sayGoodbye();    // works!
```

### Arrow functions

<!-- More on `this` later -->

Arrow functions **do not have their own `this` binding**. They borrow `this` from the parent scope.

- Perfect for callbacks or functions inside other functions (like in `.map()` or event listeners) where you don't want your `this` to play musical chair but preserve the `this` context of the surrounding code.

- Avoid them for object functions that need to refer to the object itself or for constructor functions.

```javascript
const person = {
    name: 'Alice',
    hobbies: ['coding', 'reading', 'hiking'],
    printHobbies: function() {
        // Here, 'this' refers to the 'person' object
        this.hobbies.forEach(hobby => {
            // Arrow function inherits 'this' from printHobbies. 
            console.log(`${this.name} loves ${hobby}.`);
        });
        }
};
person.printHobbies();
```

<!-- I wanna talk about rests and spread here next -->

## Arrays

Arrays in javascript are supposed to be list-like objects used to store multiple values in single variable. Javascript allows arrays to mix values of different data types. However, its a good practice to keep it homogenous for your own sanity and to avoid being judged by others (even your type checker will side-eye your PR, trust me!)

```javascript
const teamPenguinScores = [12, 24, 23, 18]     // GO! Team Penguin 
const shoppingList = ['bananas', 'yogurt', 'cocoa']  // smoothie time
const person = [48, 'Jimmy', True, "New Delhi"]  // JS allows it, but good luck remembering that sequence
```

**Valid Exception**: Arrays aren't always evil if they are mixed. Sometimes, it's convenient to stack different types together in a fixed sized array acting as a tuple where the position of the element inherently defines its type. Like React's `useState()` return array— fixed size, each slot's got a job.

```jsx
// If don't recognize the syntax here, look up 'desctructuring' next
const [count, setCount] = useState(0);  
// Using an object here would lock you into `{state, setState}` 
// or force renames like `{state:count, setState:setCount}`. Icky, right?
```

### Array Destructuring

Array Destructuring is like handling buttons on an 90s car with the labels worn-off. You gotta remember them by their positions. You miss that and the next thing you find yourself flashing Morse code to traffice like an idiot.

JavaScript allows you to unpack values from arrays (or properties from objects, which is Object Destructuring) into distinct variables. The syntax loosk similar to an array literal, just flipped sides after the halftime whistle.

```javascript
const lineup = ["ter Stegen", "Araujo", "Lewandowski", "De Jong", ]
const [fist, second] = lineup

// Rather than
let first = lineup[0];
let second = lineup[1];
```

#### Common Use Cases

```javascript
// pick what you need, skip the rest
const [fist] = lineup   // only 1st element needed
const [,,,fourth] = lineup   // only 4th need
const [first,,,fourth] = lineup   // just these two, that's all

const [first, second, ...bench]     // or you could just do this 
// Note: rest element must be the last element

// Fun fact: you don't even need a stored array for this
// Try:
let a=10, b=20;
[a,b] = [b,a]   // swap using destucturing

```

> Array destructuring works entirely on the premise that you know, and are relying on, the exact sequence and type of data at each position. Mess up the order or skip an index, and you’re holding `undefined` like a kid who just dropped his ice cream.

### Mutating vs. Non-Mutating Array Methods

An array method is a built-in declarative tools to manipulate arrays— predefined operations to perform on the array liking adding, removing or sorting values without reinventing the wheel from scratch every time. Some change the value inside (mutating), others spit out a new array without touching the original (non-mutating)

> Remember that an array or object even if declared using `const` can still be mutated. `const` only stops reassignment not mutation. Which means binding is constant, so you can not bind another reference to the variable. However, the content inside the array is perfectly mutable.

#### 1. Impure Methods (Mutating)

Impure Methods are like those annoying little cousins we all hate who graffiti your books like they own it and your parents blame you for having the books lying around. Relatable? Learn from it. Mutating array methods are notorious for rewriting the original array in place. They surely are faster than creating a new array but use 'em only when mean it.

**Examples**: `push()`, `pop()`, `shift()`, `unshift()`, `splice()`, `sort()`, `reverse()`. They alter the array directly, sometimes returning something, sometimes not.

> Note: Even when you pass an array (or object) into a function (as an argument, or as a prop in React), what gets passed is a reference to the same underlying array object in memory. So a mutating method will alter then original caller's array too because both variables point to the same object.

**Best Practice**: Confine mutators to localized operations within private scope by cloning the array first before operating.

```javascript
const orderByName = (data) => [...data].sort(...)   
```

> Pitfall: `sort()` is especially treacherous for mutating the original array. Most people confuse it as a pure method because it also returns a resultant array (chainable). Don't be like them.

#### 2. Pure Methods (Non-Mutating)

Pure Methods are the nice nerdy kids who follow rules— same input, same output, no touching the original array. They take an array, do their thing, and spit out a **new result**. No mutation. No suprises. Best to stick with pure methods unless mutation is explicitly required. This prevents "side effects" where a function unexpectedly changes data that another part of your application relies on.

**Examples**: map(), filter(), slice(), concat(), reduce(), find(), includes(), indexOf(), join(), flat().

> Note: Pure methods are chainable since each operation returns a predictable resultant array — e.g., `arr.map(...).filter(...).slice(...)` each returns a new array (with inbuilt methods), so you can keep chaining. Meanwhile chaining in case of most of the mutators such as `arr.push(4).unshift(5)` would throw a 'TypeError' because `push()` returns the new length, not an array.

```javascript
// For Fun: Creating a chainable impure method
Array.prototype.chainablePush = function(x) {
  this.push(x);   // impure: mutates original
  return this;    // returns the same array, so chainable
};

const arr = [1, 2];
arr.chainablePush(3).chainablePush(4);
console.log(arr); // [1, 2, 3, 4]
```

### JavaScript Array Methods Cheat Sheet (AI Generated)

#### **Inspection / Info:**

| Method            | Returns        | Mutates? | Chainable? | Notes                           |
|-------------------|----------------|----------|------------|---------------------------------|
| `length`          | Number         | ❌       | N/A        | Property, not a method          |
| `includes(val)`   | Boolean        | ❌       | ❌         | Checks if value exists          |
| `indexOf(val)`    | Number         | ❌       | ❌         | First index of value            |
| `lastIndexOf(val)`| Number         | ❌       | ❌         | Last index of value             |
| `find(fn)`        | Element        | ❌       | ❌         | First element matching predicate|
| `findIndex(fn)`   | Number         | ❌       | ❌         | Index of first match            |
| `every(fn)`       | Boolean        | ❌       | ❌         | All elements pass test?         |
| `some(fn)`        | Boolean        | ❌       | ❌         | Any element passes test?        |

#### **Transformation:**

| Method            | Returns        | Mutates? | Chainable? | Notes                           |
|-------------------|----------------|----------|------------|---------------------------------|
| `map(fn)`         | New array      | ❌       | ✅         | Pure transform                  |
| `filter(fn)`      | New array      | ❌       | ✅         | Keeps matching elements         |
| `flat(depth)`     | New array      | ❌       | ✅         | Flattens nested arrays          |
| `flatMap(fn)`     | New array      | ❌       | ✅         | Map + flat(1)                   |
| `slice(start, end)` | New array    | ❌       | ✅         | Extracts subarray               |
| `concat(...arrs)` | New array      | ❌       | ✅         | Joins arrays                    |
| `join(sep)`       | String         | ❌       | ❌         | Concatenates elements           |

#### **Mutation (Impure):**

| Method            | Returns            | Mutates? | Chainable? | Notes                           |
|-------------------|--------------------|----------|------------|---------------------------------|
| `push(...vals)`   | New length         | ✅       | ❌         | Adds to end                     |
| `pop()`           | Removed element    | ✅       | ❌         | Removes from end                |
| `shift()`         | Removed element    | ✅       | ❌         | Removes from start              |
| `unshift(...vals)`| New length         | ✅       | ❌         | Adds to start                   |
| `splice(start, del, ...vals)` | Removed elements | ✅       | ❌         | Insert/remove in place          |
| `sort(fn)`        | Same array         | ✅       | ✅         | Sorts in place                  |
| `reverse()`       | Same array         | ✅       | ✅         | Reverses in place               |
| `fill(val, start, end)` | Same array   | ✅       | ✅         | Overwrites values               |
| `copyWithin(target, start, end)` | Same array | ✅       | ✅         | Copies part of array            |

#### **Reduction / Aggregation:**

| Method            | Returns        | Mutates? | Chainable? | Notes                           |
|-------------------|----------------|----------|------------|---------------------------------|
| `reduce(fn, init)`| Any            | ❌       | ❌         | Left-to-right accumulator       |
| `reduceRight(fn, init)` | Any      | ❌       | ❌         | Right-to-left accumulator       |
| `toString()`      | String         | ❌       | ❌         | Comma-separated string          |
| `toLocaleString()`| String         | ❌       | ❌         | Locale-aware string             |

#### **ES2023+ Goodies:**

| Method            | Returns        | Mutates? | Chainable? | Notes                           |
|-------------------|----------------|----------|------------|---------------------------------|
| `at(index)`       | Element        | ❌       | ❌         | Supports negative indices       |
| `with(index, val)`| New array      | ❌       | ✅         | Returns copy with one element replaced |
| `toReversed()`    | New array      | ❌       | ✅         | Pure version of reverse         |
| `toSorted(fn)`    | New array      | ❌       | ✅         | Pure version of sort            |
| `toSpliced(...)`  | New array      | ❌       | ✅         | Pure version of splice          |
| `findLast(fn)`    | Element        | ❌       | ❌         | Last element matching predicate |
| `findLastIndex(fn)` | Number       | ❌       | ❌         | Index of last match             |

### Common Confusions in Array Methods

- **`slice` vs. `splice`**: The key difference is mutation. `slice(start, end)` does not mutate but returns you a new subarray with selected elements. `splice(start, deleteCount, ...itemsToAdd)` on the other hand is used to add, remove, or replace items in place.

    ```javascript
    // SLICING
    const arr = [1, 2, 3, 4, 5];
    const sliced = arr.slice(1, 4);     // [2, 3, 4]
    console.log(arr);    // [1, 2, 3, 4, 5] (unchanged)

    // SPLICING
    const arr = [1, 2, 3, 4, 5];
    const spliced = arr.splice(1, 2, 99, 100);   // [2, 3] (removed elements)
    console.log(arr);     // [1, 99, 100, 4, 5] (mutated)
    ```

- **`map` vs `forEach`**: While both iterate through each element of the array the key difference is the return value and chainablity. `map(callbackfn)` returns a **new array** with transformed values whereas `forEach(callbackfn)` only executes the callback for each element and returns nothing.

    ```js
    [1, 2, 3].map(x => x * 2);    // [2, 4, 6]
    [1, 2, 3].forEach(x => x*2); // undefined
    ```

- **`filter` vs `find`**: Both call the predicate function for each element of the array. While `filter(fn)` returns **all matching elements** in the array for which the predicate returns `true`, `find(fn) the **first matching element** only.

    ```js
    [1, 2, 3, 4, 5].filter(x => x > 2); // [3, 4, 5]
    [1, 2, 3, 4, 5].find(x => x > 2);   // 3
    ```

- **`sort` vs `toSorted` (ES2023+)**: `sort(compareFn)` mutates the original array while `toSorted(compareFn)` returns a new sorted array, leaving the original intact.

- **`includes` vs `indexOf`**: While `includes(searchElement)` returns a **boolean**, `indexOf(searchElement)` returns the **index** (or `-1` if not found).

> Note: Arrays are just specialized objects with extra internal behavior (numeric indexing, length, etc.). Try `typeof []`.

## Objects

Want to store hetrogenous values? We've got objects for that. Objects are self-explanatory if only you name the properties right. Unlike arrays, data in objects are stored as key-value pairs that keep things tidy like `{name: 'Om', job: 'Being amazing'}.` No digging through indices like a raccoon in a dumpster. No need to write a novel explaining why index 2 is a boolean but 3's a poem.

Keys are usually **strings** (or symbols), values can be anything: strings, numbers, functions, even other objects. Even if you use a number, boolean, or object as a key, it gets **coerced to a string**.

```js
// Creation
const obj = { key: "value" }; // Literal
const obj2 = new Object(); // Constructor

// Access
obj.key // Dot notation
obj["key"]  // Bracket notation
```

> Use the dot notation when the key is a valid identifier and you know it at the time of coding. The bracket notation is mostly used for dynamic access when the key is **stored in a variable**, contains spaces or special characters, or is a number.

```javascript
const propertyToAccess = 'name';
const user = { name: 'Alice', 'user-role': 'Admin' };

console.log(user.name); // 'Alice'
console.log(user[propertyToAccess]); // user['name'] -> 'Alice' (Accessing via variable)
console.log(user['user-role']); // 'Admin' (Accessing with special characters)
```

> Treat key names as variable names. Apply name rules. Consider naming conventions. Remember that dot notation only works with valid JavaScript identifiers. This means that any keys following the naming rules can be used in a dot notation. Even the reserved keywords! Look at this wierd looking object below:

```js
const obj = {
    // can be accessed by both
    id: 'a1',
    category: 'books',
    $: 55, 
    _: 55,
    true: true, // yeah this too - the engine must be resolving `obj.true` as `obj["true"]` internally for consistency
    function(){},   // what is this syntax?

    // can be accessed only through bracket notation
    1: 5, 
    '-': "1", 
    '{}': { },
    '()=>{}': () => { },
    [window]: window     // what in the world is this???
}
```

### Functions in Objects

Javascript treats functions like any other value. This means that functions can be stored, or passed on as any other value. When you store a function as a property inside an object, it's called a **method**.

```js
const peppaPig = {
    name: "Peppa",
    species: "Pig",
    age: 4,
    catchphrase: "Eee-oink!",
    isDirty: false,

    // Object Methods -> storing functions as properties
    oink: function () {
        console.log(`${this.name} goes ${this.catchphrase}`);     // this -> current object
    }

    // Modern (ES6) Syntax
    jumpInPuddle(){
        console.log("Sploosh!")
        this.isDirty = true;
    }
};
```

Now, you can tell what `function(){}` meant up there. We used `function` as the key identifier (reserved words are valid) and used the ES6 syntax to create it a function that does nothing.

**Can I use an arrow function here?** Definitely, if you keep in mind that an arrow function does not have its own this binding. Instead, it inherits this from its parent scope. In the above case, `this` means the `Window` object or whatever your global environment is called in your runtime. So use them only when you want to preserve the `this` of its lexical scope.

```js
let counter = {
    count: 0,
    start() {
    // 'this' here means the 'counter' object.
    setInterval(() => {
        this.count++;   // this inheritred from the `start()` method
    }, 1000);
    }
};
```

**Invoking the methods**: Methods tied to an object can be accessed like any other value and invoked like any other function.

```js
peppaPig.oink();            // dot notation
peppaPig['jumpInPuddle']()  // bracket notation    
// Note that `peppaPig['jumpInPuddle']` is used to first access the value of the property, which returns a function and then it's called using `()`.
```

> **Best Practice**: Always type check your methods like `typeof peppaPig['oink'] === "function"`  before you call them, especially when dealing with external, dynamic, user-driven code like `typeof peppaPig[selectedAction] === "function"`, if you don't want a `TypeError` beaming off your screen.

<!-- Talk about optional chaining and computed keys -->

## Everything Is An Object

...

### Functions As An Objext

Functions in JS are fundamentally callable objects. Functions have an internal `[[Call]]` slot, which the JS engine uses when you invoke them. Moreover, Functions in JS are first-class objects. This means they can:

1. Be stored in a variable.
2. Be passed as an argument.
3. Be returned from another function.
4. Be assigned as an object property.
5. Be constructed dynamically (new Function()).

<!-- Talk about autoboxing and wrapper objects -->
