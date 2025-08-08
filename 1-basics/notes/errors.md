# **Javascript Fundamentals**

## **Errors**

An `Error` in JavaScript is a structured object. When you `catch (e)`, `e` is an instance of the `Error` class (or one of its subclasses) with the following properties:

* **`name`**: A string indicating the error type (e.g., `'TypeError'`, `'ReferenceError'`).
* **`message`**: A human-readable string describing what went wrong for developers.
* **`stack`**: A highly detailed, multi-line string showing the "call stack"—the sequence of function calls that led to the error. The stack trace is generated **at the moment the `Error` object is created**, not when it's thrown.
* **`cause` (Modern JS - ES2022)**:  The `cause` property allows you to wrap an error within another error. This is crucial for maintaining context in complex systems.

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

* **Examples**: `TypeError` (calling `null.method()`), `ReferenceError` (accessing an undeclared variable), passing a string where a number is required.
* **How to Handle**: **You don't.** You should let the application crash (or restart in a controlled way, like with a Node.js process manager).
* **The Goal**: The error log is your feedback. **Fix the code.** Programmer errors are not runtime problems to be handled gracefully; they are development-time problems to be eliminated.

#### 2. Operational Errors (Exceptions)

These are expected, albeit unsuccessful, events that occur at runtime due to factors outside your direct code's logic. Your code is correct, but the world it interacts with is not.

  * **Examples**: The network connection is down (API call fails), a file you're trying to read doesn't exist, the user enters invalid input, a third-party service is rate-limiting you.
  * **How to Handle**: **This is what `try...catch` was made for.** You anticipate these failures and build logic to handle them: retry the connection, show a friendly message to the user, fall back to cached data.
  * **The Goal**: Maintain the program's integrity and provide a good user experience despite external failures.

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

### **Ways To Log Errors**

### `console.error()`: Logging an Error

* **Purpose**: Used to **log** error messages to the console.
* **Effect**: Does **not stop** code execution.
* **Use Case**: Helpful for debugging or tracking issues **without interrupting the flow**.
* **Example**:

  ```javascript
  console.error("Something went wrong");
  console.log("This still runs");
  ```

-----

### `throw`: Throwing an Error

* **Purpose**: Used to **intentionally raise** an error.
* **Effect**: **Immediately halts** execution unless caught with `try...catch`.
* **Use Case**: When you want to enforce error handling or signal that something went wrong.
* **Example**:

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

---

### 🔍 Summary Table

| Type                     | Stops Execution | Can Be Caught | Purpose                      |
|--------------------------|-----------------|----------------|------------------------------|
| `console.error()`        | ❌ No            | ❌ Not needed  | Logging/debugging            |
| `throw`                  | ✅ Yes           | ✅ Yes         | Manual error signaling       |
| Interpreter Errors       | ✅ Yes           | ✅ Yes         | Automatic error detection    |

-----

You'd typically use `throw` for critical failures and `console.error()` for diagnostics.