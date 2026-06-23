# 🌳 Tree Shaking — Remove Dead Code Automatically

> **In plain English:** Tree shaking removes JavaScript code you imported but never actually used.

---

## What Is Tree Shaking?

Imagine a library has 100 functions. You only use 3.
Tree shaking removes the other 97 from your final bundle automatically.

```javascript
// You write:
import { debounce } from 'lodash-es'

// Tree shaker sees you only use debounce
// Final bundle contains: only debounce (2KB), not all of lodash (70KB)
```

---

## Requirements: ES Modules Only

Tree shaking only works with ES Module syntax (`import`/`export`).
It does NOT work with CommonJS (`require`/`module.exports`).

```javascript
// ES Modules — tree shakeable
import { specific } from 'library'
export function myFunc() {}

// CommonJS — NOT tree shakeable
const { specific } = require('library')
module.exports = { myFunc }
```

---

## How to Make Sure Tree Shaking Works

### 1. Use named imports
```javascript
// GOOD: Bundler knows exactly what you use
import { Button, Input } from 'ui-library'
```

### 2. Use lodash-es instead of lodash
```javascript
// BAD: CommonJS lodash — tree shaking does not work
import { debounce } from 'lodash'

// GOOD: ES Module lodash — fully tree shakeable
import { debounce } from 'lodash-es'
```

### 3. Mark package as side-effect-free
In your `package.json`:
```json
{
  "sideEffects": false
}
```

Or for specific files only:
```json
{
  "sideEffects": ["*.css", "./src/polyfills.js"]
}
```

---

## Verify Tree Shaking Works

Run the bundle analyzer and check if a library takes more space than expected:
```bash
npm run analyze
```

If `lodash` still shows up as 70KB even though you only import one function, it is not tree shaking — switch to `lodash-es`.
