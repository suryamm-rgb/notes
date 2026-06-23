# ⏱️ LCP — Largest Contentful Paint

> **In plain English:** How fast does the biggest visible thing on your page appear?

---

## What Is LCP?

LCP measures the time from when a user first navigates to a page until the **largest image or text block** is rendered and visible in the viewport (the visible screen area).

### What counts as the "largest element"?
- `<img>` tags
- `<image>` inside an SVG
- `<video>` poster images
- Block-level elements with large background images
- Large text blocks (`<h1>`, `<p>`, etc.)

---

## 🎯 LCP Targets

| Score | Time |
|-------|------|
| 🟢 Good | ≤ 2.5 seconds |
| 🟡 Needs Improvement | 2.5 – 4.0 seconds |
| 🔴 Poor | > 4.0 seconds |

---

## 🔍 How to Find Your LCP Element

### Using Chrome DevTools:
1. Open your site in Chrome
2. Press `F12` → go to **Performance** tab
3. Click **Record** → reload the page → stop recording
4. Look for the **LCP** marker in the timeline
5. The highlighted element IS your LCP element

### Using PageSpeed Insights:
1. Go to https://pagespeed.web.dev
2. Paste your URL
3. Scroll to **Opportunities** section
4. It will highlight your LCP element with a screenshot

---

## ❌ Common Causes of Bad LCP (and How to Fix Them)

### Problem 1: Large, unoptimized hero image
**The most common issue on portfolio sites.**

```html
<!-- ❌ BAD: Raw large image, no optimization -->
<img src="/hero-photo.jpg" alt="My photo" />

<!-- ✅ GOOD: Optimized with Next.js Image component -->
import Image from 'next/image'

<Image
  src="/hero-photo.jpg"
  alt="My photo"
  width={1200}
  height={600}
  priority          {/* ← This tells the browser to load it FIRST */}
  quality={85}
/>
```

### Problem 2: Image not preloaded (browser discovers it too late)

```html
<!-- ✅ Add this to your <head> for hero images NOT using next/image -->
<link rel="preload" as="image" href="/hero-photo.jpg" />
```

### Problem 3: Slow server response (bad TTFB)
If your server is slow, LCP will always be slow. Fix TTFB first.
→ See `TTFB.md`

### Problem 4: Render-blocking JavaScript
JavaScript that runs before painting blocks everything.

```javascript
// ❌ BAD: Heavy library loaded synchronously
import heavyLibrary from 'heavy-library' // 500KB loaded upfront

// ✅ GOOD: Load only when needed
const heavyLibrary = await import('heavy-library') // Loaded on demand
```

### Problem 5: Slow fonts delaying text
```css
/* ✅ Add font-display: swap so text shows immediately */
@font-face {
  font-family: 'MyFont';
  src: url('/fonts/myfont.woff2') format('woff2');
  font-display: swap; /* Shows fallback font first, swaps when loaded */
}
```

---

## ✅ LCP Optimization Checklist for Portfolios

- [ ] Hero image uses `next/image` with `priority` prop
- [ ] Hero image is compressed to < 200KB (use WebP format)
- [ ] No render-blocking scripts in `<head>`
- [ ] Fonts use `font-display: swap`
- [ ] Server responds in < 600ms (TTFB)
- [ ] No large CSS files blocking paint

---

## 📊 Real Example: Portfolio Before & After

| | Before | After |
|---|--------|-------|
| Hero image size | 2.4MB JPEG | 180KB WebP |
| LCP time | 5.2s 🔴 | 1.8s 🟢 |
| What changed | Added `next/image` with `priority` | — |

---

## 🛠️ Tools to Measure LCP

| Tool | How to Use |
|------|-----------|
| PageSpeed Insights | https://pagespeed.web.dev |
| Chrome DevTools | F12 → Performance tab |
| Lighthouse | F12 → Lighthouse tab |
| Web Vitals Chrome Extension | Install from Chrome Web Store |

---

*Next: Read `INP.md` →*
