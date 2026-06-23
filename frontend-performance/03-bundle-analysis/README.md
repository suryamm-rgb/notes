# 03 — Bundle Analysis: Making Your JavaScript Smaller

When a user visits your site, the browser has to **download all your JavaScript** before the page becomes interactive. The bigger your JS, the slower your site. This is the #1 reason modern sites feel sluggish.

This folder teaches you how to **see what's in your JS** and **make it smaller**.

---

## What is a "bundle"?

When you build your site, tools like **Vite**, **Webpack**, or **Next.js** take all your source files (`.js`, `.ts`, `.tsx`, CSS, images) and combine them into a few output files called **bundles**. The browser downloads these bundles.

A typical React site bundle contains:
- Your code
- React + ReactDOM (~45 KB gzipped)
- Routing library (~10 KB)
- UI libraries (Material UI, Tailwind utilities, etc.)
- Icon packs (can be HUGE if imported wrong)
- Analytics / tracking scripts

If your bundle is 2 MB, a user on slow 3G waits **10+ seconds** just for the JS.

---

## How big should my bundle be?

| Bundle size (gzipped) | Verdict |
|-----------------------|---------|
| < 100 KB | 🟢 Excellent |
| 100–300 KB | 🟢 Good |
| 300–500 KB | 🟠 Acceptable |
| 500 KB – 1 MB | 🔴 Slow |
| > 1 MB | 🔴 Very slow |

Aim for **under 200 KB gzipped** for the initial load.

---

## 1. JavaScript Bundle Size — how to measure it

### For Vite projects
```bash
npm install -D rollup-plugin-visualizer
```

In `vite.config.ts`:
```ts
import { visualizer } from "rollup-plugin-visualizer";

export default defineConfig({
  plugins: [
    visualizer({ open: true, gzipSize: true, brotliSize: true })
  ]
});
```

Run `npm run build` — a treemap opens showing every package's size.

### For Webpack projects
```bash
npm install -D webpack-bundle-analyzer
```
```js
// webpack.config.js
const { BundleAnalyzerPlugin } = require('webpack-bundle-analyzer');
module.exports = {
  plugins: [new BundleAnalyzerPlugin()]
};
```

### For Next.js
```bash
npm install -D @next/bundle-analyzer
```
```js
// next.config.js
const withBundleAnalyzer = require('@next/bundle-analyzer')({
  enabled: process.env.ANALYZE === 'true'
});
module.exports = withBundleAnalyzer({});
```
Run: `ANALYZE=true npm run build`

### Online tool (no setup)
Paste a package name at <https://bundlephobia.com/> to see its size *before* installing it.

---

## 2. Code Splitting

**Code splitting = breaking your bundle into multiple smaller chunks** that load only when needed.

Instead of one 1 MB bundle on every page, split it into:
- `main.js` (50 KB) — needed everywhere
- `dashboard.js` (200 KB) — only on /dashboard
- `editor.js` (300 KB) — only on /editor

Users only download what they actually visit.

### How (React + Vite/Webpack)
Use `React.lazy()` for route-level splitting:
```tsx
import { lazy, Suspense } from 'react';

const Dashboard = lazy(() => import('./Dashboard'));

function App() {
  return (
    <Suspense fallback={<div>Loading...</div>}>
      <Dashboard />
    </Suspense>
  );
}
```

### How (Next.js)
Use `next/dynamic`:
```tsx
import dynamic from 'next/dynamic';
const HeavyChart = dynamic(() => import('./HeavyChart'), { ssr: false });
```

---

## 3. Tree Shaking

**Tree shaking = automatically removing code you import but never use.**

Modern bundlers do this automatically, BUT only if you import correctly:

```js
// ❌ BAD — imports the entire lodash library (~70 KB)
import _ from 'lodash';
_.debounce(...);

// ✅ GOOD — imports only debounce (~2 KB)
import debounce from 'lodash/debounce';

// ✅ BEST — use a tree-shakable alternative
import { debounce } from 'lodash-es';
```

### Common offenders
| Library | Bad import | Good import |
|---------|-----------|-------------|
| lodash | `import _ from 'lodash'` | `import debounce from 'lodash/debounce'` |
| moment.js | (use date-fns instead!) | `import { format } from 'date-fns'` |
| Material UI | `import { Button } from '@mui/material'` | usually ok, but check |
| Icon libraries | `import * as Icons from 'react-icons'` | `import { FaHome } from 'react-icons/fa'` |

---

## 4. Dynamic Imports

Load code only when an event happens (button click, modal open, etc.):

```js
button.addEventListener('click', async () => {
  const { default: Chart } = await import('chart.js');
  // Chart only downloaded when user clicks
});
```

Perfect for:
- Modal/dialog content
- Charts that appear after scroll
- Admin-only features
- Rich text editors
- Video players

---

## 5. Other size-reduction tricks

- **Use modern formats**: serve `.mjs` ES modules to modern browsers
- **Drop polyfills** for old browsers if you don't need to support IE11
- **Self-host fonts** instead of Google Fonts (or use `font-display: swap`)
- **Remove dead dependencies**: run `npx depcheck` to find unused packages
- **Replace heavy libraries**:
  - `moment` → `date-fns` or native `Intl.DateTimeFormat`
  - `axios` → native `fetch`
  - `lodash` → native JS / individual functions

---

## ✅ Beginner action steps

1. Run a bundle analyzer on your project.
2. Find the **3 largest packages** in your bundle.
3. Ask yourself: *do I really need this?* Can I replace it with a smaller one or use the native browser API?
4. Switch heavy imports to specific imports (tree-shaking).
5. Add `React.lazy` for any page/route that isn't your homepage.
6. Re-run Lighthouse — your Performance score should jump.

**Next:** `04-image-optimization/README.md` — images are usually even bigger than JS!
