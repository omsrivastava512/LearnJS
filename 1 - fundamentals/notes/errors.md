# **Javascript Core**

- [**Errors**](#errors)
  - [Programmer vs. Operational Errors](#programmer-vs-operational-errors)
    - [Programmer Errors (Bugs)](#1-programmer-errors-bugs)
    - [Operational Errors (Exceptions)](#2-operational-errors-exceptions)
  - [Asynchronous Errors](#asynchronous-errors)
  - [Advanced Pattern: Custom, Semantic Errors](#advanced-pattern-custom-semantic-errors)
- [**Ways To Log Errors**](#ways-to-log-errors)
  - [`console.error()`: Logging an Error](#consoleerror-logging-an-error)
  - [`throw`: Throwing an Error](#throw-throwing-an-error)
  - [JavaScript Interpreter Errors](#javascript-interpreter-errors)
  - [Summary Table](#summary-table)
- [**Advanced Error Handling Strategies**](#advanced-error-handling-strategies)
  - [The `finally` Block: Cleanup](#the-finally-block-cleanup)
  - [Error Boundaries:](#error-boundaries)
  - [Global Uncaught Exceptions & Unhandled Rejections](#global-uncaught-exceptions--unhandled-rejections)
  - [Error Normalization: Creating a Common Language for Failure](#error-normalization-creating-a-common-language-for-failure)
- [**Types of Errors**](#types-of-errors)
  - [SyntaxError](#syntaxerror)
  - [ReferenceError](#referenceerror)
  - [TypeError](#typeerror)
  - [RangeError](#rangeerror)
  - [URIError](#urierror)

## **Errors**

An `Error` in JavaScript is a structured object. When you `catch (e)`, `e` is an instance of the `Error` class (or one of its subclasses) with the following properties:

- **`name`**: A string indicating the error type (e.g., `'TypeError'`, `'ReferenceError'`).
- **`message`**: A human-readable string describing what went wrong for developers.
- **`stack`**: A highly detailed, multi-line string showing the "call stack"—the sequence of function calls that led to the error. The stack trace is generated **at the moment the `Error` object is created**, not when it's thrown.
- **`cause` (Modern JS - ES2022)**:  The `cause` property allows you to wrap an error within another error. This is crucial for maintaining context in complex systems.

**Example of Error Wrapping:**

```javascript
async function fetchUser(userId) {
  try {
    const data = await db.query('SELECT * FROM users WHERE id = ?', userId);
    // ...
  } catch (dbError) {
    // Don't just throw the cryptic dbError. Wrap it!
    throw new Error('Failed to fetch user data.', { cause: dbError });
  }
}

try {
  await fetchUser(123);
} catch (e) {
  console.log(e.message); // "Failed to fetch user data."
  console.log(e.cause.message); // "Connection refused" or some other specific DB error.
  // Now you have both the high-level application context and the low-level root cause.
}
```

-----

### **Programmer vs. Operational Errors**

#### 1. Programmer Errors (Bugs)

These are mistakes in your code. They represent a state that should **never happen**. You violated the contract of a function or a language rule.

- **Examples**: `TypeError` (calling `null.method()`), `ReferenceError` (accessing an undeclared variable), passing a string where a number is required.
- **How to Handle**: **You don't.** You should let the application crash (or restart in a controlled way, like with a Node.js process manager).
- **The Goal**: The error log is your feedback. **Fix the code.** Programmer errors are not runtime problems to be handled gracefully; they are development-time problems to be eliminated.

#### 2. Operational Errors (Exceptions)

These are expected, albeit unsuccessful, events that occur at runtime due to factors outside your direct code's logic. Your code is correct, but the world it interacts with is not.

- **Examples**: The network connection is down (API call fails), a file you're trying to read doesn't exist, the user enters invalid input, a third-party service is rate-limiting you.
- **How to Handle**: **This is what `try...catch` was made for.** You anticipate these failures and build logic to handle them: retry the connection, show a friendly message to the user, fall back to cached data.
- **The Goal**: Maintain the program's integrity and provide a good user experience despite external failures.

-----

### **Asynchronous Errors**

A standard `try...catch` block only works for **synchronous code** within its scope. This is because the `try` block finishes executing and is removed from the call stack before the asynchronous operation completes.

**The Wrong Way:**

```javascript
try {
  setTimeout(() => {
    throw new Error("This won't be caught!");
  }, 1000);
} catch (e) {
  // This code will never run.
  console.log("Caught it!");
}
```

**The Right Ways:**

1. **For Promises (`.then/.catch`):** Use the `.catch()` method, which is specifically designed to handle promise rejections.

    ```javascript
    fetch('https://invalid-url')
      .then(res => res.json())
      .catch(err => {
        // This is the correct way to catch network or parsing errors.
        console.error("API call failed:", err);
      });
    ```

2. **For `async/await`:** `async/await` allows `try...catch` to work across asynchronous operations because it "pauses" the function execution until the promise settles.

    ```javascript
    async function fetchData() {
      try {
        const response = await fetch('https://invalid-url');
        const data = await response.json();
        return data;
      } catch (err) {
        // This works perfectly!
        console.error("API call failed:", err);
      }
    }
    ```

-----

### **Advanced Pattern: Custom, Semantic Errors**

Don't just rely on generic `Error` objects. Create your own error types to make your application's failure states explicit and easy to handle.

```javascript
// Define custom error types for your application's domain.
class ValidationError extends Error {
  constructor(message) {
    super(message);
    this.name = 'ValidationError';
  }
}

class APIAuthError extends Error {
  constructor(message) {
    super(message);
    this.name = 'APIAuthError';
  }
}

// Now you can handle errors with precision.
try {
  // some function that can throw different kinds of operational errors
  const result = await performAction(userInput);
} catch (e) {
  if (e instanceof ValidationError) {
    // Show a message to the user to fix their input.
    alert(`Invalid input: ${e.message}`);
  } else if (e instanceof APIAuthError) {
    // Redirect the user to the login page.
    window.location.href = '/login';
  } else {
    // For any other unexpected operational error, show a generic message.
    alert('An unexpected error occurred. Please try again later.');
  }
}
```

## **Ways To Log Errors**

### `console.error()`: Logging an Error

- **Purpose**: Used to **log** error messages to the console.
- **Effect**: Does **not stop** code execution.
- **Use Case**: Helpful for debugging or tracking issues **without interrupting the flow**.
- **Example**:

  ```javascript
  console.error("Something went wrong");
  console.log("This still runs");
  ```

-----

### `throw`: Throwing an Error

- **Purpose**: Used to **intentionally raise** an error.
- **Effect**: **Immediately halts** execution unless caught with `try...catch`.
- **Use Case**: When you want to enforce error handling or signal that something went wrong.
- **Example**:

  ```javascript
  throw new Error("Critical failure");
  console.log("This won't run unless caught");
  ```

-----

### JavaScript Interpreter Errors

- **Purpose**: These are **automatic errors** thrown by the JS engine.
- **Effect**: Stops execution unless handled.
- **Types**:
  - **SyntaxError**: Code structure is invalid.
  - **ReferenceError**: Accessing undefined variables.
  - **TypeError**: Using a value in an invalid way.
- **Example**:

  ```javascript
  let x = y + 1; // ReferenceError: y is not defined
  ```

-----

### Summary Table

| Type                     | Stops Execution | Can Be Caught | Purpose                      |
|--------------------------|-----------------|----------------|------------------------------|
| `console.error()`        | ❌ No            | ❌ Not needed  | Logging/debugging            |
| `throw`                  | ✅ Yes           | ✅ Yes         | Manual error signaling       |
| Interpreter Errors       | ✅ Yes           | ✅ Yes         | Automatic error detection    |

-----

You'd typically use `throw` for critical failures and `console.error()` for diagnostics.

-----

## Advanced Error Handling Strategies

### **The `finally` Block: Cleanup**

Code inside a `finally` block is **guaranteed to execute** after the `try` and `catch` blocks, regardless of whether an error was thrown or not.

Its purpose isn't to handle the error, but to perform cleanup. This prevents your application from being left in an inconsistent state.

- **Common Use Cases:**
  - Stopping a loading spinner.
  - Closing a file or database connection.
  - Releasing a resource.
  - Closing an event listener or steam reader

**For example:** A UI loading state.

```javascript
showLoadingSpinner();

try {
  // This might succeed or fail
  const data = await fetchData();
  processData(data);
} catch (err) {
  console.error("Failed to fetch data:", err);
  showErrorMessage("We couldn't get your data.");
} finally {
  // This will ALWAYS run, ensuring the spinner doesn't get stuck on screen.
  hideLoadingSpinner();
}
```

Without `finally`, you'd have to call `hideLoadingSpinner()` in both the `try` and `catch` blocks, which is repetitive and error-prone.

-----

### **Error Boundaries:**

In modern component-based applications (like React), a single error in a minor component (like a misformatted date in a calendar widget) can crash the entire application. Errors naturally have a tendency to bubble up to the parent blocks or functions untill they are caught.

An **Error Boundary** is a component that catches errors in its child components, logs them, and displays a fallback UI instead of letting the entire app crash.

- **How it Works:** Wrapping sections of your UI in a special "boundary" component. If any component inside that boundary throws an error during rendering, the boundary's special lifecycle hooks (`getDerivedStateFromError` and `componentDidCatch` in React) are triggered.
- **The Purpose:** This is the ultimate implementation of handling **operational errors** gracefully. You accept that a part of the UI might fail (e.g., due to bad API data), and you prevent that failure from destroying the user's entire session. The user sees a message like "Sorry, this widget couldn't load," while the rest of the application remains fully interactive.

-----

### Global Uncaught Exceptions & Unhandled Rejections

What happens if an error is thrown but no `try...catch` block ever catches it? It becomes a global error.

1. **Uncaught Exception:** A synchronous error that was never caught.
2. **Unhandled Rejection:** A Promise that was rejected, but no `.catch()` handler was ever attached to it.

You can listen for these globally to act as a "final safety net."

**In the Browser:**

  ```javascript
  window.onerror = (message, source, lineno, colno, error) => {
    console.log("An uncaught exception occurred!");
    // Send this error to your logging service
  };

  window.onunhandledrejection = event => {
    console.log(`An unhandled promise rejection occurred: ${event.reason}`);
    // Send this event.reason to your logging service
  };
  ```

**In Node.js:**

```javascript
process.on('uncaughtException', (err, origin) => {
  console.error(`Caught exception: ${err}\n` + `Exception origin: ${origin}`);
  // Log the error, then exit. Do not resume!
  process.exit(1);
});
```

**Log and Die.** When an uncaught exception occurs, your application is in an undefined state. A memory resource might be corrupted, or a file handle left open. The only safe thing to do is to log the error in detail and shut down the process. A process manager (like PM2 or a Docker orchestrator) should then restart it in a clean state. **Never try to resume an application after an uncaught exception.**

-----

### **Error Normalization: Creating a Common Language for Failure**

In a large application, especially one with microservices, errors come from dozens of sources: database drivers, third-party APIs, HTTP clients, user input validation, etc. Each one has its own error format.

Error Normalization is the practice of catching these varied errors and converting them into a single, consistent format for your application. You might define a standard internal error structure that your entire system uses.

Example of a normalized error:

``` javascript
{
  name: 'DatabaseError', // Your custom, high-level error name.
  message: 'Could not retrieve user profile.', // A user-safe message.
  code: 'DB_CONN_FAILED', // An internal, machine-readable code.
  severity: 'critical', // For alerting (info, warning, error, critical).
  isOperational: true, // Follows the philosophy we discussed.
  cause: { ... } // The original error object from the database driver.
}
```

## Types of Errors

### SyntaxError

This is an error in the code's structure that violates the rules of JavaScript's grammar. It's unique because it's thrown during the initial **parsing phase**, before any of your code is executed.

- **When and How It Occurs:** The engine's parser scans your code to understand it. If it encounters a sequence of characters that doesn't conform to valid JavaScript syntax, it stops immediately and throws a `SyntaxError`.
  - A typo like a missing parenthesis, bracket, or brace: `console.log("hello")`
  - An invalid assignment: `let 1a = "invalid";`
  - Using a reserved word as a variable name: `let let = 5;`
  - A misplaced keyword, like `await` outside of an `async` function.

- **Impact:** **Fatal to script execution.** If a `<script>` tag contains a `SyntaxError`, none of the code within that script will run. The browser or Node.js environment won't even attempt to execute it because it couldn't be successfully parsed into an executable format.

-----

### ReferenceError

This error occurs when you try to use a variable that doesn't exist or hasn't been initialized yet. It's an error of memory access or scope.

- **When and How It Occurs:** This happens during **runtime**.

  - Accessing a variable that was never declared: `console.log(myUndeclaredVar);`
  - A simple typo in a variable name: `let userName = "Alex"; console.log(userNmae);`
  - Accessing a `let` or `const` variable before its declaration (within the **Temporal Dead Zone** or TDZ):

    ```javascript
    console.log(myVar); // ReferenceError: Cannot access 'myVar' before initialization
    let myVar = 10;
    ```

- **Impact:** It immediately stops the execution of the current code block. In a simple script, this will halt the entire program. In a larger application, it will stop the function it occurred in, and the error will propagate up the call stack until it's caught or crashes the application.

-----

### TypeError

This is arguably the most common runtime error in JavaScript. It occurs when an operation is performed on a value of the wrong data type. The syntax is correct, the variable exists, but the **type** of the value is incompatible with the operation.

- **When and How It Occurs:** During **runtime**.
  - Trying to access a property on `null` or `undefined`: This is the classic `TypeError`. An API call fails and returns `null`, but your code still tries to do `result.data`.
  - Calling something that is not a function: `let name = "Alex"; name();`
  - Using an array method on a non-array: `let user = { name: "Alex" }; user.map(u => ...);`
  - Attempting to reassign a `const` variable.

- **Impact:** Like a `ReferenceError`, it halts the current execution path and propagates up the call stack. This error almost always signifies a **programmer error (a bug)**. Your code made an incorrect assumption about the state or type of a variable.

-----

### RangeError

This error is thrown when a numeric value is not within its allowed range or set of values.

- **When and How It Occurs:** During **runtime**, usually in specific numerical or array operations.

  - Creating an array with an invalid length: `new Array(-1);` or `new Array(Infinity);`
  - Passing an invalid value to number formatting functions: `(12.345).toFixed(-1);` or `(10).toPrecision(500);`
  * Deep recursion that leads to a stack overflow, which is technically a `RangeError` in many environments because the call stack size has exceeded its allowed range.

- **Impact:** Halts execution. It's less common than a `TypeError` but often points to logical errors in algorithms or data processing where numbers are calculated incorrectly before being used in a range-sensitive function.

-----

### URIError

This is a more specialized error that occurs when one of the global URI-handling functions is used incorrectly.

- **When and How It Occurs:** During **runtime**, when you pass a malformed or illegal string to a URI function.
  - `decodeURIComponent('%');` // A single '%' is not a valid start of an encoded sequence.
  - `decodeURI('https://example.com/%%invalid');`

- **Impact:** Halts execution. This is typically an **operational error**, often caused by receiving corrupted or improperly encoded data from an external source, like a URL query parameter.
