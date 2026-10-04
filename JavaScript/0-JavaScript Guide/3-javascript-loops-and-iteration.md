# JavaScript — Loops and Iteration (Key Points)

Loops repeat an action a given number of times (possibly zero). JavaScript provides several loop mechanisms suited to different situations:

- `for`
- `do...while`
- `while`
- `labeled statement`
- `break`
- `continue`
- `for...in`
- `for...of`

## 1. `for` statement
Repeats until a condition is `false`.

```js
for (initialization; condition; afterthought)
  statement
```

**Execution order:**
1. `initialization` runs once (can declare variables).
2. `condition` is checked — if `true`, run the loop; if omitted, treated as `true`.
3. `statement` executes (use `{ }` for multiple statements).
4. `afterthought` runs.
5. Back to step 2.

```js
function countSelected(selectObject) {
  let numberSelected = 0;
  for (let i = 0; i < selectObject.options.length; i++) {
    if (selectObject.options[i].selected) numberSelected++;
  }
  return numberSelected;
}
```

## 2. `do...while` statement
Runs the statement **at least once**, then repeats while the condition is `true`.

```js
do
  statement
while (condition);
```

```js
let i = 0;
do {
  i += 1;
  console.log(i);
} while (i < 5);
```

## 3. `while` statement
Checks the condition **before** each execution.

```js
while (condition)
  statement
```

```js
let n = 0, x = 0;
while (n < 3) {
  n++;
  x += n;
}
// n=1,x=1 → n=2,x=3 → n=3,x=6 → loop stops
```

> **Warning — infinite loops:** always ensure the condition can become `false`.
```js
while (true) {
  console.log("Hello, world!"); // never stops
}
```

## 4. Labeled statement
Gives a statement an identifier, referenced later by `break`/`continue`.

```js
label:
  statement
```
- `label` must be a valid identifier (not a reserved word).

## 5. `break` statement
Terminates a loop, `switch`, or a labeled statement.

```js
break;        // terminates innermost loop/switch
break label;  // terminates the specified labeled statement
```

```js
// Find index of theValue
for (let i = 0; i < a.length; i++) {
  if (a[i] === theValue) break;
}
```

```js
// Breaking to a label
let x = 0, z = 0;
labelCancelLoops: while (true) {
  x += 1;
  z = 1;
  while (true) {
    z += 1;
    if (z === 10 && x === 10) break labelCancelLoops;
    else if (z === 10) break;
  }
}
```

## 6. `continue` statement
Skips to the **next iteration** of a loop instead of ending it entirely.
- `while`: jumps back to the condition check.
- `for`: jumps to the increment (afterthought) expression.

```js
continue;        // applies to innermost loop
continue label;  // applies to the labeled loop
```

```js
let i = 0, n = 0;
while (i < 5) {
  i++;
  if (i === 3) continue; // skip when i is 3
  n += i;
  console.log(n);
}
// Logs: 1 3 7 12
```

Labeled example — `continue` inside `checkJ` only restarts the inner loop, not the outer `checkIandJ`:
```js
let i = 0, j = 10;
checkIandJ: while (i < 4) {
  i += 1;
  checkJ: while (j > 4) {
    j -= 1;
    if (j % 2 === 0) continue; // restarts checkJ
    console.log(j, "is odd.");
  }
}
```

## 7. `for...in` statement
Iterates a variable over all **enumerable property names** of an object.

```js
for (variable in object)
  statement
```

```js
function dumpProps(obj, objName) {
  let result = "";
  for (const i in obj) {
    result += `${objName}.${i} = ${obj[i]}<br>`;
  }
  return result;
}
```

> **Caution with arrays:** `for...in` also returns **user-defined properties**, not just numeric indexes. **Prefer a traditional `for` loop with a numeric index for arrays.**

## 8. `for...of` statement
Iterates over the **values** of an iterable (`Array`, `Map`, `Set`, `arguments`, etc.).

```js
for (variable of iterable)
  statement
```

**`for...of` vs `for...in`:**
```js
const arr = [3, 5, 7];
arr.foo = "hello";

for (const i in arr) console.log(i);
// "0" "1" "2" "foo"   ← property names, includes custom props

for (const i of arr) console.log(i);
// 3 5 7               ← property values only
```

Can be combined with **destructuring**, e.g. iterating `Object.entries()`:
```js
const obj = { foo: 1, bar: 2 };
for (const [key, val] of Object.entries(obj)) {
  console.log(key, val);
}
// "foo" 1
// "bar" 2
```

## Quick Reference

| Statement | Checks condition | Iterates over | Notes |
|---|---|---|---|
| `for` | Before each pass | Counter-based | Classic C-style loop |
| `do...while` | After each pass | — | Always runs at least once |
| `while` | Before each pass | — | Watch for infinite loops |
| `for...in` | — | Property **names** | Avoid for arrays |
| `for...of` | — | Property **values** | Works with any iterable |
| `break` | — | — | Exits loop/switch (optionally by label) |
| `continue` | — | — | Skips to next iteration (optionally by label) |
