# ✂️ Code Splitting

> **In plain English:** Instead of loading ALL your JavaScript at once, split it into pieces and load each piece only when needed.

---

## The Problem Without Code Splitting

```
User visits homepage → Downloads 1MB of JS
(Including code for /projects, /about, /contact that they have not visited yet)
```

## The Solution With Code Splitting

```
User visits homepage  → Downloads 200KB (just homepage code)
User visits /projects → Downloads 50KB  (just projects code)
```

---

## Automatic Code Splitting in Next.js

Next.js does this automatically for routes. Every file in the `app/` directory gets its own JS chunk.

```
app/page.js           → chunk-homepage.js
app/about/page.js     → chunk-about.js
app/projects/page.js  → chunk-projects.js
```

You get this for free — no configuration needed.

---

## Manual Code Splitting with dynamic()

For heavy components inside a page:

```javascript
import dynamic from 'next/dynamic'

// Static import: always in the bundle
import HeavyChart from './HeavyChart'

// Dynamic import: its own separate chunk, loaded on demand
const HeavyChart = dynamic(() => import('./HeavyChart'), {
  loading: () => <div>Loading chart...</div>,
  ssr: false   // Do not render on server (for browser-only libraries)
})
```

### Portfolio example: animation library
```javascript
// Heavy animation library only loads on the About page
const AnimatedTimeline = dynamic(() => import('../components/AnimatedTimeline'), {
  loading: () => <p>Loading timeline...</p>
})
```

---

## Route vs Component vs Interaction Splitting

| Type | Trigger | How |
|------|---------|-----|
| Route-based | Page navigation | Automatic in Next.js |
| Component-based | Component mounts | `dynamic()` |
| Interaction-based | User clicks/acts | Manual `await import()` |

```javascript
// Interaction-based: load CSV parser only when user uploads a file
async function handleUpload(file) {
  const { parse } = await import('csv-parser')
  return parse(file)
}
```
