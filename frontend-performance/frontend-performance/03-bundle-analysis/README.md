# 📦 Bundle Analysis — What Is a JavaScript Bundle?

> **In plain English:** When you write React/Next.js code, it gets packed into one or more JavaScript files called "bundles." Smaller bundles = faster websites.

---

## What Is a JavaScript Bundle?

When you write code across many files:
```
src/
  App.jsx
  components/Header.jsx
  components/Footer.jsx
  pages/Home.jsx
  utils/helpers.js
```

A **bundler** (Webpack, Turbopack) combines them into files the browser can load:
```
dist/
  main.js          (your app code)
  vendor.js        (libraries like React)
  chunk-abc123.js  (lazy-loaded route)
```

---

## Why Bundle Size Matters

| Bundle Size | Load Time (3G) | User Impact |
|-------------|----------------|-------------|
| 100KB | ~0.5s | Great |
| 500KB | ~2.5s | Acceptable |
| 1MB | ~5s | Poor |
| 3MB | ~15s | Many users leave |

JavaScript is expensive — the browser downloads it, parses it, and executes it. All of that blocks rendering.

---

## The 4 Main Techniques

| Technique | What It Does | File |
|-----------|-------------|------|
| Code Splitting | Breaks bundle into smaller pieces | `CODE-SPLITTING.md` |
| Tree Shaking | Removes code you don't use | `TREE-SHAKING.md` |
| Dynamic Imports | Loads code only when needed | `DYNAMIC-IMPORTS.md` |
| Bundle Analysis | Visualizes what is in your bundle | `WEBPACK-ANALYZER.md` / `NEXTJS-ANALYZER.md` |

---

## Quick Win: Check Your Bundle Size RIGHT NOW

```bash
# In any Next.js project
npm run build
```

Look at the "First Load JS" column. If any route shows > 200KB — investigate with the bundle analyzer.
