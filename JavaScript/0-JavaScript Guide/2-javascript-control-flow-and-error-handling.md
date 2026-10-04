# JavaScript — Control Flow and Error Handling (Key Points)

JavaScript has a compact set of control flow statements. Statements are separated by semicolons (`;`), and any expression is also a statement.

## 1. Block Statement
- Groups statements using curly braces `{ }`.
- Commonly used with `if`, `for`, `while`.

```js
while (x < 10) {
  x++;
}
```

> **Gotcha:** `var` is **not block-scoped** — it's scoped to the function/script, so effects persist after the block. Use `let` or `const` to avoid this.

```js
var x = 1;
{
  var x = 2;
}
console.log(x); // 2 (would be 1 in C or Java)
```

## 2. Conditional Statements

### `if...else`
```js
if (condition1) {
  statement1;
} else if (condition2) {
  statement2;
} else {
  statementLast;
}
```
- Only the **first** true condition runs.
- **Best practices:**
  - Always use block statements `{ }`, especially when nesting.
  - Avoid assignments as conditions (`if (x = y)`).

### Falsy values
Evaluate to `false`:

| Value |
|---|
| `false` |
| `undefined` |
| `null` |
| `0` |
| `NaN` |
| `""` (empty string) |

- **Everything else is truthy**, including all objects.
- Beware the `Boolean` object:

```js
const b = new Boolean(false);
if (b) { /* runs: object is truthy */ }
if (b == true) { /* does NOT run */ }
```

### `switch`
```js
switch (expression) {
  case label1:
    statements1;
    break;
  case label2:
    statements2;
    break;
  default:
    statementsDefault;
}
```
- Matches the expression to a `case` label and runs its statements.
- No match → runs `default` (if present); otherwise continues after the `switch`.
- `default` is conventionally last, but doesn't have to be.
- **`break`** exits the `switch`. **If omitted, execution falls through** to the next `case`.

## 3. Exception Handling

### `throw`
- Throws **any** expression (string, number, boolean, object).
- Better to throw dedicated types: **ECMAScript exceptions** (e.g., `Error`) or **`DOMException`**.

```js
throw "Error2";        // String
throw 42;              // Number
throw new Error("Invalid month code");
```

### `try...catch`
- **`try`** block: statements to attempt.
- **`catch`** block: runs if an exception is thrown in `try` (or in any function called from it); skipped otherwise.
- **`finally`** block: runs after `try`/`catch`, before the code that follows.

```js
try {
  monthName = getMonthName(myMonth);
} catch (e) {
  monthName = "unknown";
  logMyErrors(e);
}
```

### The `catch` block
```js
catch (exception) {
  statements
}
```
- The identifier holds the thrown value and exists **only within the `catch` block**.
- Use **`console.error()`** (not `console.log()`) for logging errors.

### The `finally` block
- Runs **whether or not an exception is thrown**, even if no `catch` handles it.
- Ideal for releasing resources (e.g., closing files).

```js
openMyFile();
try {
  writeMyFile(theData);
} catch (e) {
  handleError(e);
} finally {
  closeMyFile(); // always runs
}
```

> **Gotcha:** A `return` in `finally` **overrides** any `return` or `throw` from `try`/`catch`.

```js
function f() {
  try {
    throw "bogus";
  } catch (e) {
    throw e;          // suspended...
  } finally {
    return false;     // ...and overwritten by this
  }
}
console.log(f()); // false (the throw is swallowed)
```

### Nesting `try...catch`
- An inner `try` without a `catch` **must** have a `finally`.
- The enclosing `try...catch`'s `catch` block is then checked for a match.

### Using `Error` objects
- **`name`** — general class of error (`Error`, `DOMException`, ...).
- **`message`** — concise description.
- Use `new Error("message")` for your own exceptions so these properties are available.

```js
try {
  doSomethingErrorProne();
} catch (e) {
  console.error(e.name);    // 'Error'
  console.error(e.message); // 'The message'
}
```

## Quick Reference

| Statement | Purpose |
|---|---|
| `{ }` | Group statements |
| `if...else` | Run code based on a condition |
| `switch` | Match a value against multiple cases |
| `throw` | Raise an exception |
| `try...catch` | Handle exceptions |
| `finally` | Cleanup code that always runs |
