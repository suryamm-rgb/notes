# JavaScript — Grammar and Types (Key Points)

## 1. Basics
- Syntax borrowed mostly from **Java, C, C++**; also influenced by Awk, Perl, Python.
- **Case-sensitive** and uses **Unicode** (`Früh` ≠ `früh`).
- Instructions are called **statements**, separated by semicolons (`;`).
- Semicolons are optional on their own line (ASI – automatic semicolon insertion), but **best practice: always use them** to reduce bugs.
- Source is scanned left to right into tokens, control characters, line terminators, comments, and whitespace.

## 2. Comments
```js
// one-line comment

/* multi-line
 * comment
 */
```
- **Block comments cannot be nested** (a stray `*/` ends the comment). Escape it: `*\/`.
- Comments behave like whitespace.
- **Hashbang** comment (`#!/usr/bin/env node`) at the start of a file specifies the engine to run the script.

## 3. Declarations

| Keyword | Meaning |
|---|---|
| `var` | Declares a variable (function/global scoped), optional initializer |
| `let` | Block-scoped variable, optional initializer |
| `const` | Block-scoped, cannot be re-assigned, **must** be initialized |
| `using` | Like `const`, synchronously disposed |
| `await using` | Like `const`, asynchronously disposed |

### Identifier rules
- Start with a letter, `_`, or `$`; later characters may also be digits.
- Unicode letters allowed (`å`, `ü`).
- Valid examples: `Number_hits`, `temp99`, `$credit`, `_name`.

### Declaration vs. initialization
- `let x = 42` → `let x` is the **declaration**, `= 42` is the **initializer**.
- No initializer → value is `undefined`.
- `const x;` → **SyntaxError** (missing initializer).
- Always declare variables before use; assigning to undeclared variables creates an accidental global (an **error in strict mode**).
- Destructuring: `const { bar } = foo;`

### Variable scope
- **Global** (script mode), **Module**, **Function**, and **Block** (`let`/`const` only).
- `let`/`const` are block-scoped; **`var` is not** — it leaks out of blocks to the function/global scope.

```js
if (true) { var x = 5; }
console.log(x); // 5

if (Math.random() > 0.5) { const y = 5; }
console.log(y); // ReferenceError
```

### Hoisting
- **`var`**: declaration is hoisted, value is not → reads as `undefined` before assignment.
- **`let` / `const`**: in the **temporal dead zone** from block start until declaration → `ReferenceError` if accessed early.
- **Function declarations**: hoisted entirely; can be called anywhere in scope.
- Best practice: place `var` statements near the top of the function.

### Global variables
- Properties of the global object.
- `window` in browsers; **`globalThis`** works in all environments.
- Accessible across frames, e.g. `parent.phoneNumber`.

### Constants
- `const PI = 3.14;` — read-only binding; can't be re-assigned or re-declared.
- Can't share a name with a function/variable in the same scope.
- **`const` prevents re-assignment, not mutation:**

```js
const MY_OBJECT = { key: "value" };
MY_OBJECT.key = "otherValue";      // OK

const MY_ARRAY = ["HTML", "CSS"];
MY_ARRAY.push("JAVASCRIPT");       // OK
```

## 4. Data Types
**Seven primitives + Object** (functions are a kind of object):

| Type | Notes |
|---|---|
| Boolean | `true`, `false` |
| null | Null keyword (case-sensitive) |
| undefined | Value not defined |
| Number | Integer or floating point (`42`, `3.14159`) |
| BigInt | Arbitrary-precision integer (`9007199254740992n`) |
| String | Text (`"Howdy"`) |
| Symbol | Unique and immutable |
| Object | Named containers for values |

### Dynamic typing & conversion
- No need to declare types; types convert automatically.
- **`+` with string and number → string concatenation:**
```js
"The answer is " + 42; // "The answer is 42"
"37" + 7;              // "377"
```
- **Other operators convert to numbers:**
```js
"37" - 7; // 30
"37" * 7; // 259
```

### Strings → numbers
- `parseInt()`, `parseFloat()`, `Number()`, or unary `+`.
- `parseInt` returns whole numbers only; **always pass the radix**: `parseInt("101", 2); // 5`
- `(+"1.1") + (+"1.1"); // 2.2`

## 5. Literals

### Array literals
```js
const coffees = ["French Roast", "Colombian", "Kona"];
```
- A new array is created each time the literal is evaluated (e.g., each function call).
- **Extra commas** create empty slots (not the same as `undefined`; skipped by methods like `map`, but `arr[i]` returns `undefined`).
  - `["Lion", , "Angel"]` → length 3, one empty item
  - Trailing comma is ignored: `["home", , "school", ,]` → length 4
- Trailing commas keep git diffs clean; but for clarity, declare missing items explicitly as `undefined` or add a comment.

### Boolean literals
- `true` and `false` — don't confuse with the `Boolean` **object** wrapper.

### Numeric literals
- Must be unsigned per spec (`-123.4` is unary `-` applied to `123.4`).

| Base | Prefix | Example |
|---|---|---|
| Decimal | none | `117` |
| Octal | `0` or `0o` | `0o777` |
| Hex | `0x` | `0x1123` |
| Binary | `0b` | `0b11` |
| BigInt | trailing `n` | `123n`, `0o123n` (leading-zero octal like `0123n` not allowed) |

- **Floating-point:** `[digits].[digits][(E|e)[(+|-)]digits]` → `3.1415926`, `.123456789`, `3.1E+12`, `.1e-23`

### Object literals
```js
const car = { myCar: "Saturn", getCar: carTypes("Honda"), special: sales };
const nested = { manyCars: { a: "Saab", b: "Jeep" }, 7: "Mazda" };
```
- **Don't start a statement with an object literal** — `{` is parsed as a block.
- Property names can be any string; non-identifier names need quotes and **bracket access**:
```js
const o = { "": "empty", "!": "Bang!" };
o[""];  // "empty"
o["!"]; // "Bang!"
```

### Enhanced object literals
Shorthand features: `__proto__` at construction, `foo` shorthand for `foo: foo`, method definitions, `super` calls, computed property names.
```js
const obj = {
  __proto__: theProtoObj,
  handler,                       // shorthand
  toString() { return `d ${super.toString()}`; },
  ["prop_" + (() => 42)()]: 42,  // computed name
};
```

### RegExp literals
```js
const re = /ab+c/;
```

### String literals
- Single or double quotes (must match): `'foo'`, `"bar"`.
- Use literals over `String` objects; methods/`.length` work via temporary wrapper.

#### Template literals (backticks)
```js
`Hello ${name}, how are you ${time}?`   // interpolation
`multi
 line`                                   // multiline
```

#### Tagged templates
- A function name before a template literal: `print`...``
- Tag function receives `(segments, ...args)` — a cleaner alternative to formatter functions.
- Equivalent to a plain call: `print(["I need to do:\n", ...], todos, progress)`.

### Special characters in strings

| Char | Meaning |
|---|---|
| `\0` | Null byte |
| `\b` | Backspace |
| `\f` | Form feed |
| `\n` | New line |
| `\r` | Carriage return |
| `\t` | Tab |
| `\v` | Vertical tab |
| `\'` `\"` | Quotes |
| `\\` | Backslash |
| `\XXX` | Latin-1 char via up to 3 octal digits (0–377) |
| `\xXX` | Latin-1 char via 2 hex digits (00–FF) |
| `\uXXXX` | Unicode char via 4 hex digits |
| `\u{XXXXX}` | Unicode code point escape |

- Backslash before an unlisted character is ignored (**deprecated**).
- Escape quotes: `"He read \"The Cremation of Sam McGee\""`
- Literal backslash: `"c:\\temp"`
- Backslash + line break continues a string across lines (both removed from value).

## 6. Related Topics (Next Chapters)
- Control flow and error handling
- Loops and iteration
- Functions
- Expressions and operators
