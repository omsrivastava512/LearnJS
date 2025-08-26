# JavaScript Naming Conventions (AI Assisted)

- [Casing Styles](#casing-styles)
- [Naming by Identifier Type](#naming-by-identifier-type)
  - [Variables](#variables)
  - [Boolean Variables](#boolean-variables)
  - [Functions and Methods](#functions-and-methods)
  - [Classes and Constructor Functions](#classes-and-constructor-functions)
  - [Constants](#constants)
  - ["Private" Members](#private-members)
- [General Best Practices (AI Generated)](#general-best-practices-ai-generated)
- [Naming Conflicts](#naming-conflicts)
  - [The Global `window` Object Problem](#the-global-window-object-problem)
  - [The DOM Element `id` Problem](#the-dom-element-id-problem)
  - [The Form Element Property Problem](#the-form-element-name-problem)
- [Reserved Words](#reserved-words)
  - [JavaScript Language Reserved Words](#javascript-language-reserved-words)
    - [Always Reserved](#always-reserved)
    - [Reserved for Future Use](#reserved-for-future-use)
  - [Environment-Specific "Reserved" Globals](#environment-specific-reserved-globals)
    - [In the Browser (DOM & Window)](#in-the-browser-dom--window)
    - [In Node.js](#in-nodejs)
- [Library-Specific "Reserved" Words (e.g., React)](#library-specific-reserved-words-eg-react)
  - [In React](#in-react)
  - [In Next.js](#in-nextjs)

Naming conventions are a set of guidelines, not strict rules enforced by the compiler, for naming identifiers like variables, functions, and classes. Adhering to a consistent naming strategy is crucial for producing **readable, maintainable, and professional code**.

The core principle is to name things based on what they are or what they do, making the code self-documenting.

-----

## Casing Styles

JavaScript primarily uses three casing conventions, each with a specific purpose.

- **camelCase**: This is the most common convention in JavaScript. The first word is lowercase, and the first letter of every subsequent word is capitalized.
  - **Use for**: Variables, functions, methods, and properties.
  - **Example**: `let userProfile;`, `function calculateTotalPrice() {}`

- **PascalCase (or UpperCamelCase)**: Every word, including the first, starts with a capital letter.
  - **Use for**: Classes, Constructor Functions, and Components (in frameworks like React). This convention visually signals that something is a "blueprint" or a constructor that should be instantiated with the `new` keyword.
  - **Example**: `class UserManager {}`, `function User(name) {}`

- **SCREAMING\_SNAKE\_CASE**: All letters are uppercase, with words separated by underscores.
  - **Use for**: Constants; values that are "magic" or are intended to never be reassigned throughout the program's lifecycle. This casing makes them stand out, shouting "I am a constant, do not try to change me\!"
  - **Example**: `const MAX_LOGIN_ATTEMPTS = 5;`, `const API_KEY = '...';`

-----

## Naming by Identifier Type

### Variables

Variables should be named using **camelCase**. The name should be a descriptive noun that clearly indicates the data it holds.

**Why?** Clarity and readability. A name like `u` is meaningless, whereas `currentUser` is immediately understandable.

✅ `let firstName = 'John';`, `const itemsInCart = [];`, `let isLoading = false;`

❌ `let n = 'John';`, `const list = [];`, `let l = false;`

### Boolean Variables

For variables that hold a boolean (`true`/`false`) value, prefix the name with `is`, `has`, or `can`.

**Why?** This makes expressions in conditional statements read like plain English, improving readability.

✅ `if (isOpen) {...}`, `while (!hasPermission) {...}`, `const canBeDeleted = true;`

❌ `if (open) {...}`, `while (!permission) {...}`, `const delete = true;` (Note: `delete` is also a reserved keyword).

### Functions and Methods

Functions should be named using **camelCase**. The name should be a verb or a verb phrase that describes the action the function performs.

**Why?** Functions do things. Their names should reflect their action. This helps predict what a function will do and what it might return without reading its implementation.

✅ `function getUser()`, `function calculateSalesTax()`, `function renderUserProfilePage()`

❌ `function user()`, `function salesTax()`, `function profilePage()`

### Classes and Constructor Functions

Classes and constructor functions must be named using **PascalCase**.

**Why?** This is a critical convention. `PascalCase` acts as a strong visual cue that you are dealing with a blueprint, not a regular function or object instance. It signals to other developers that it should be instantiated using the `new` keyword (e.g., `new UserManager()`).

✅ `class ProductManager {}`, `function AdminUser(name) {...}`

❌ `class productManager {}`, `function admin_user(name) {...}`

### Constants

Constants—variables declared with `const` whose values should never be changed—must use **SCREAMING\_SNAKE\_CASE**. This applies especially to "magic numbers" or configuration settings.

**Why?** This makes constants instantly recognizable. It warns developers that this value is a fundamental, fixed part of the application's logic.

✅ `const PI = 3.14159;`, `const API_URL = 'https://api.example.com';`, `const SECONDS_IN_DAY = 86400;`

❌ `const pi = 3.14159;`, `const apiUrl = '...';`

### "Private" Members

JavaScript now has true private class fields using the `#` prefix. However, for a long time, the convention for "private" methods or properties (those not intended for use outside the class/object) has been a leading underscore `_`.

**Reasoning**: The underscore `_` serves as a "do not touch" signal to other developers. It communicates that a property or method is an internal implementation detail and could change without warning. The modern `#` syntax enforces this privacy at the language level.

- **Convention (old)**:

```javascript
class ApiService {
    _apiKey = 'secret'; // Conventionally private

    _connect() {
    // Internal logic
    }
}
```

- **Modern Standard (true private)**:

```javascript
class ApiService {
    #apiKey = 'secret'; // Truly private

    #connect() {
    // Internal logic, cannot be called from outside
    }
}
```

-----

## General Best Practices (AI Generated)

- **Be Descriptive, Not Abbreviated**: Code is read far more often than it is written. Opt for clarity over brevity.
  - ✅ `const destinationCity = 'London';`
  - ❌ `const dest = 'London';`
- **Avoid Redundant Context**: The object or class already provides context.
  - ✅ In a `User` class: `this.name`, `this.address`, `this.save()`
  - ❌ In a `User` class: `this.userName`, `this.userAddress`, `this.saveUser()`
- **Use Pronounceable and Searchable Names**: If you can't say it out loud, it's a bad name. Single-letter variables (except for simple loop counters like `i`) are hard to search for in a codebase.
  - ✅ `let queryTimeout = 5000;`
  - ❌ `let qto = 5000;`
- **Prioritize Specificity Over Generality**: Strive for names that are as specific as possible.
  - ✅ `function validateAndSanitizeUserInput(formData) { ... }`
  - ❌ `function processData(data) { ... }`
  - ✅ `let userToDeactivate = getUser();`
  - ❌ `let temp = getUser();`
- **Indicate Units**: Include units when dealing with numbers that have units.
  - ✅ `const timeoutInMs = 5000;`, `const elementWidthInPx = 100;`
  - ❌  `const timeout = 5000;`, `const elementWidth = 100;`
- **Avoid "Noise" Words and Redundant Prefixes**: Common offenders include Data, Info, Manager, Util, Handler, and redundant type prefixes like str or arr or sufixes like List or Group.
  - ✅ `const userData = { name: 'User' };`, `const userInfo = { ... }`
  - ❌ `const user = { name: 'User' };`
  - ✅ `let users = [...]`, `let customers = [...]`, `const arrNumber = [1, 2];`
  - ❌ `let userList = [...]`, `let customerGroup = [...]`, `const numbers = [1,2]`
- **Be Consistent**: If you call something `getUser` in one place, don't call a similar function `fetchUser` in another. Pick a vocabulary and stick to it.
- **Match Name Length to Variable Scope**: The larger the scope, the more descriptive its name needs to be.

-----

## Naming Conflicts

Naming conflicts and reserved words are critical hurdles in JavaScript that can cause subtle, hard-to-debug errors. Understanding them is key to writing robust code.

The main issue is that JavaScript code often runs in a shared environment, like a browser's global `window` object, which already has many predefined properties and methods. Using common but generic names can unintentionally overwrite or shadow these built-in functionalities.

-----

### The Global `window` Object Problem

In browsers, the global scope is the `window` object. Using `var` to declare a variable in the global scope attaches it as a property to `window`. This is a major source of conflicts.

- **`name`**: The `window.name` property is a standard browser feature used to get/set the name of a browsing context.
  - **Conflict Example**:

    ```javascript
    // This not only overwrites the browser's window.name functionality but also coerce its type if assigned a different data type, potentially causing runtime errors.
    var name = 'User';
    console.log(window.name); // Outputs "User", which is now a string, not the window name property.
    ```

  - **Why it's bad**: Any script that relies on the original `window.name` functionality will now fail or behave unexpectedly.
- **Other Common `window` Properties**: You should avoid using these as global variables.
  - **`top`**, **`parent`**, **`self`**: Used for navigating frames.
  - **`location`**: An object containing URL information (`location.href`, `location.reload()`). Overwriting this breaks navigation.
  - **`length`**: Represents the number of frames in the window.
  - **`status`**, **`history`**, **`opener`**, **`event`** (older browsers).

**Solution**: Modern JavaScript largely solves this. **Use `let` and `const`** for declarations. They have block scope and **do not** attach to the `window` object, even when declared globally.

```javascript
let name = 'User'; // This is fine
console.log(window.name); // Outputs the original window.name property, not "User"
```

### The DOM Element `id` Problem

In many browsers, for backward compatibility, any DOM element with an `id` attribute automatically becomes a global variable.

- **Conflict Example**:

  ```html
  <div id="userProfile"></div>
  ```

  ```javascript
  // In the console, you can directly access `userProfile`
  console.log(userProfile); // Outputs the <div> element

  // This creates a conflict:
  const userProfile = { name: 'Ram' }; // Now `userProfile` refers to your object, shadowing the DOM element.
  ```

- **Why it's bad**: It's an unreliable and implicit behavior. Always use explicit selectors like **`document.getElementById('userProfile')`** to access DOM elements.

### The Form Element `name` Problem

When you give an `<input>` a `name` attribute, it becomes a property on the parent `<form>` element. This can conflict with the form's own properties.

- **Conflict Example**:

  ```html
  <form id="myForm">
    <input name="id" value="user-123">
    <input name="value" value="some-text">
    <input name="submit"> </form>
  ```

  ```javascript
  const myForm = document.getElementById('myForm');

  console.log(myForm.id); // Outputs "user-123" (the input's value), NOT "myForm" (the form's id).
  console.log(myForm.value); // Outputs "some-text" (the second input's value), which is confusing.
  console.log(typeof myForm.submit); // "object" (the input element), NOT "function" (the submit method).
  ```
  
- **Why it's bad**: It breaks built-in form functionality. You can't programmatically call `myForm.submit()` anymore. Always use the **`form.elements`** collection to access controls by name to avoid these conflicts: `myForm.elements['submit']`.

-----

## Reserved Words

These are words that have special meaning in the JavaScript language. You **cannot** use them as identifiers (variable names, function names, etc.). Attempting to do so will result in a `SyntaxError`.

### JavaScript Language Reserved Words

This list is defined by the ECMAScript standard.

#### Always Reserved

These words are currently used as keywords in the language.

|             |             |             |             |
| :---------- | :---------- | :---------- | :---------- |
| `await`     | `break`     | `case`      | `catch`     |
| `class`     | `const`     | `continue`  | `debugger`  |
| `default`   | `delete`    | `do`        | `else`      |
| `export`    | `extends`   | `false`     | `finally`   |
| `for`       | `function`  | `if`        | `import`    |
| `in`        | `instanceof`| `new`       | `null`      |
| `return`    | `super`     | `switch`    | `this`      |
| `throw`     | `true`      | `try`       | `typeof`    |
| `var`       | `void`      | `while`     | `with`      |
| `yield`     |             |             |             |

#### Reserved for Future Use

These words are not currently used but are reserved for potential future versions of JavaScript. It's best to avoid them completely.

| `enum`        | `implements` | `interface` |
| :------------ | :----------- | :---------- |
| `package`     | `private`    | `protected` |
| `public`      | `static`     | `let`\* |

*\*`let` is now implemented but is still on the official reserved list for historical and contextual reasons.*

### Environment-Specific "Reserved" Globals

These aren't language keywords, but they are predefined global variables in specific environments. Overwriting them will break functionality.

#### In the Browser (DOM & Window)

- **`window`**: The global object itself.
- **`document`**: The entry point to the DOM.
- **`console`**: The debugging console object.
- **`alert`**, **`prompt`**, **`confirm`**: User interaction functions.
- **`fetch`**: The modern API for making network requests.
- **`setTimeout`**, **`setInterval`**, **`clearTimeout`**, **`clearInterval`**: Asynchronous timer functions.
- **`location`**, **`history`**, **`navigator`**: Browser and page state objects.
- **`screen`**, **`localStorage`**, **`sessionStorage`**: Browser feature APIs.

#### In Node.js

- **`process`**: Provides information about, and control over, the current Node.js process.
- **`require`**: The function used to import modules in CommonJS.
- **`module`**: A reference to the current module.
- **`exports`**: An object which is exposed when a file is imported via `require`.
- **`__dirname`**: The directory name of the current module.
- **`__filename`**: The file name of the current module.
- **`global`**: The global object in Node.js (analogous to `window` in browsers).
- **`Buffer`**: Used to handle binary data.

## Library-Specific "Reserved" Words (e.g., React)

Frameworks often have their own set of special property names that have a specific meaning and shouldn't be used for your own data.

### In React

- **`key`**: A special string attribute you need to include when creating lists of elements. React uses it to identify which items have changed, are added, or are removed. You cannot access it via `this.props.key`.
- **`ref`**: Provides a way to access DOM nodes or React elements created in the `render` method.
- **`children`**: A special prop, `this.props.children`, that contains the content passed between the opening and closing tags of a component.
- **Lifecycle Methods**: You should not name your own methods with the same names as React's lifecycle methods, as it would override their functionality (e.g., `render`, `constructor`, `componentDidMount`, `shouldComponentUpdate`, etc.).
- **State and Props**: The property names **`state`** and **`props`** are fundamental to how React components work and should not be used for other purposes on a component instance.

### In Next.js

Next.js, a framework for React, has a powerful file-based routing system. Many of its "reserved" words are special function exports or file names that Next.js recognizes to enable features like Server-Side Rendering (SSR) and Static Site Generation (SSG).

#### Data Fetching & Page Configuration (Recognized Exports)

These are special `async` functions you export from a page file to fetch data before rendering.

- **`getStaticProps`**: (Pages Router) Fetches data at **build time**. The page is pre-rendered as static HTML. You cannot use this with `getServerSideProps`.
- **`getStaticPaths`**: (Pages Router) Used with dynamic routes to specify which paths should be pre-rendered at build time. It must be used with `getStaticProps`.
- **`getServerSideProps`**: (Pages Router) Fetches data on **every request**. The page is rendered on the server for each user visit. You cannot use this with `getStaticProps`.
- **`generateStaticParams`**: (App Router) The equivalent of `getStaticPaths`. It's used in dynamic route segments to generate static pages at build time.
- **Route Segment Config**: (App Router) Variables exported from a page or layout to configure its behavior.
  - **`dynamic`**: Controls whether a route is static or dynamic (e.g., `'force-dynamic'`, `'force-static'`).
  - **`revalidate`**: Sets the revalidation time for a page in seconds (Incremental Static Regeneration).
  - **`fetchCache`**: Controls the default caching behavior of `fetch` requests within the route segment.

#### Special File and Folder Names

Next.js uses a strict file-naming convention, especially in the modern **App Router**.

- **Special Folders**:
  - **`app/`**: The root of the App Router. All UI is defined here.
  - **`pages/`**: The root of the older Pages Router.
  - **`public/`**: For static assets like images and fonts. Files here are served from the root URL.
  - **`api/`**: Used inside `pages/` or `app/` (as a `route.js` file) to create API endpoints.

- **Special Files (in App Router)**:
  - **`layout.js` / `layout.tsx`**: Defines a shared UI wrapper for a segment and its children.
  - **`page.js` / `page.tsx`**: The main UI for a unique route.
  - **`loading.js` / `loading.tsx`**: Creates instant loading UI (a "spinner") that's shown while page content loads.
  - **`not-found.js` / `not-found.tsx`**: Renders the UI when the `notFound()` function is thrown or a URL doesn't exist.
  - **`error.js` / `error.tsx`**: Handles unexpected errors and displays a fallback UI.
  - **`template.js` / `template.tsx`**: Similar to `layout`, but it re-mounts on navigation, not preserving state.
  - **`route.js` / `route.ts`**: Used to create API endpoints within the App Router.
