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

- *Function Declarations*

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

### Arrow Functions

> More on `this` later

Arrow functions **do not have their own `this` binding**. They borrow `this` from the parent scope.

- Perfect for callbacks or methods inside other functions (like in `.map()` or event listeners) where you don't want your `this` to play musical chair but preserve the `this` context of the surrounding code.

- Avoid them for object methods that need to refer to the object itself or for constructor functions.

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

> I wanna talk about rests and spread here next

## Arrays

Arrays in javascript are supposed to be list-like objects used to store multiple values in single variable. Javascript allows arrays to mix values of different data types. However, its a good practice to keep it homogenous for your own sanity and to avoid being judged by others (even your type checker will side-eye your PR, trust me!)

```javascript
const scores = [12, 24, 23, 18]     // lovely
const items = ['bananas', 'milk', 'cocoa']  // yummy
const person = [48, 'Jimmy', True]  // JS allows it, but good luck remembering the sequence
```

**Exception**: Arrays aren't always evil if they are mixed. Sometimes, it's convenient to stack different types together in a fixed sized array acting as a tuple where the position of the element inherently defines its type. Like React's `useState()` return array— fixed size, each slot's got a job.

```jsx
// If don't recognize the syntax here, look up 'desctructuring'
const [count, setCount] = useState(0);  
// Using an object here would lock you into `{state, setState}` 
// or force renames like `{state:count, setState:setCount}`. Icky, right?
```

## Objects

Want to store hetrogenous values? We've got objects for that. Objects are self-explanatory if only you name the properties right. Unlike arrays, no need write a novel explaining why index 2 is a Boolean but 3's a poem.

> Talk about reasignment vs mutation
