# JavaScript Naming Conventions (AI Assisted)

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
- **Indicate Units and Data Structures**: 
  - ✅ `const timeoutInMs = 5000;`, `const elementWidthInPx = 100;`
  - ❌  `const timeout = 5000;`, `const elementWidth = 100;`
  - ✅ `let users = [...]`, `let customers = [...]`
  - ❌ `let userList = [...]`, `let customerGroup = [...]`
- **Be Consistent**: If you call something `getUser` in one place, don't call a similar function `fetchUser` in another. Pick a vocabulary and stick to it.
- **Match Name Length to Variable Scope**: The larger the scope, the more descriptive its name needs to be.

-----
