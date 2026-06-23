# ⚡ Performance Audits — How to Fix Every Issue

> This file covers the most common Lighthouse performance warnings and exactly how to fix each one.

---

## 🖼️ "Serve images in next-gen formats"

**What it means:** Your images are JPEG/PNG. You should use WebP or AVIF instead — they're much smaller.

**Size comparison for the same quality:**
```
JPEG:  500KB
WebP:  200KB  (60% smaller)
AVIF:  120KB  (76% smaller)
```

**Fix with Next.js (automatic):**
```jsx
import Image from 'next/image'

// next/image automatically converts to WebP/AVIF
<Image src="/hero.jpg" alt="Hero" width={1200} height={600} />
```

**Fix without Next.js (manual):**
- Go to https://squoosh.app (free, online)
- Upload your image
- Choose "WebP" output format
- Download and replace your original file

---

## 🖼️ "Properly size images"

**What it means:** You're loading a 2000px wide image but displaying it at 400px.

```jsx
// ❌ BAD: Loading huge image, displaying small
<img src="/photo-2000x1500.jpg" style={{ width: '400px' }} />

// ✅ GOOD: Use next/image with correct sizes
<Image
  src="/photo.jpg"
  alt="Photo"
  width={400}
  height={300}
  sizes="(max-width: 768px) 100vw, 400px"  // Tells browser what size to load
/>
```

---

## 🚫 "Eliminate render-blocking resources"

**What it means:** You have `<script>` or `<link>` tags in `<head>` that block the page from painting.

**Fix scripts:**
```html
<!-- ❌ BAD: Blocks rendering -->
<script src="/analytics.js"></script>

<!-- ✅ GOOD: Deferred (runs after page loads) -->
<script src="/analytics.js" defer></script>

<!-- ✅ ALSO GOOD: Async (runs whenever it's ready) -->
<script src="/analytics.js" async></script>
```

**Fix CSS:**
```html
<!-- ✅ GOOD: Inline critical CSS, load rest asynchronously -->
<style>
  /* Only the CSS needed for above-the-fold content */
  body { font-family: sans-serif; }
  .hero { ... }
</style>
<link rel="preload" href="/styles.css" as="style" onload="this.rel='stylesheet'" />
```

---

## 📦 "Remove unused JavaScript"

**What it means:** You're loading JavaScript that isn't used on this page.

```javascript
// ❌ BAD: Importing entire library when you only need one function
import _ from 'lodash'
const result = _.debounce(myFunc, 300)

// ✅ GOOD: Import only what you need
import debounce from 'lodash/debounce'
const result = debounce(myFunc, 300)
```

**For Next.js:** Use dynamic imports for components not needed on initial load:
```javascript
import dynamic from 'next/dynamic'

// This component's code only loads when it's needed
const HeavyChart = dynamic(() => import('./HeavyChart'), {
  loading: () => <p>Loading chart...</p>
})
```

---

## 🅰️ "Ensure text remains visible during webfont load"

```css
/* ✅ Add font-display: swap to all @font-face declarations */
@font-face {
  font-family: 'Inter';
  src: url('/fonts/inter.woff2') format('woff2');
  font-display: swap;  /* Show fallback font immediately, swap when ready */
}
```

**In Next.js with Google Fonts:**
```javascript
// app/layout.js
import { Inter } from 'next/font/google'

const inter = Inter({
  subsets: ['latin'],
  display: 'swap',  // This is the key option
})
```

---

## 🖼️ "Image elements do not have explicit width and height"

```html
<!-- ❌ BAD: No dimensions = layout shifts when image loads -->
<img src="/photo.jpg" alt="Photo" />

<!-- ✅ GOOD: Explicit dimensions = space reserved before image loads -->
<img src="/photo.jpg" alt="Photo" width="800" height="600" />
```

---

## ⚡ "Preload Largest Contentful Paint image"

```html
<!-- Add this in <head> for your hero image -->
<link rel="preload" as="image" href="/hero.webp" />
```

In Next.js with `next/image`, use the `priority` prop instead:
```jsx
<Image src="/hero.jpg" alt="Hero" width={1200} height={600} priority />
```

---

## 📊 Quick Fix Priority Table

| Lighthouse Warning | Typical Savings | Difficulty |
|-------------------|----------------|------------|
| Serve next-gen images | 0.5–2s | Easy |
| Properly size images | 0.5–1.5s | Easy |
| Eliminate render-blocking | 0.3–1s | Medium |
| Remove unused JS | 0.2–0.8s | Medium |
| Preload LCP image | 0.2–0.5s | Easy |
| Reduce unused CSS | 0.1–0.3s | Hard |

---

*Next: Read `ACCESSIBILITY.md` →*
