# 01 — Core Web Vitals

**Core Web Vitals** are the 3 metrics Google uses to decide if your site provides a "good experience." They directly affect your Google search ranking.

Think of them as a **report card from Google** about your site's speed and stability.

---

## The 3 Core Web Vitals

| Metric | What it measures | Good | Needs work | Poor |
|--------|------------------|------|-----------|------|
| **LCP** | Loading speed | ≤ 2.5s | ≤ 4.0s | > 4.0s |
| **INP** | Interactivity | ≤ 200ms | ≤ 500ms | > 500ms |
| **CLS** | Visual stability | ≤ 0.1 | ≤ 0.25 | > 0.25 |

Plus one extra metric that's not "Core" but matters a lot:

| Metric | What it measures | Good |
|--------|------------------|------|
| **TTFB** | Server response speed | ≤ 0.8s |

---

## 1. LCP — Largest Contentful Paint

### What is it?
**LCP = how long until the biggest visible thing on the page appears.**

Usually this is your hero image, a big headline, or a banner. If your hero image takes 5 seconds to load, your LCP is 5 seconds. Bad.

### Why it matters
Users perceive your site as "loaded" when the biggest thing appears. If that takes too long, they leave.

### Common causes of bad LCP
- Huge unoptimized images (a 5MB photo when 200KB would do)
- Slow server (TTFB problem — see below)
- Render-blocking JavaScript or CSS
- Web fonts loading slowly and blocking text

### How to fix it
- **Compress your hero image** (use WebP/AVIF format, max ~200KB)
- **Add `loading="eager"` and `fetchpriority="high"`** to the hero image
- **Preload the hero image** in `<head>`:
  ```html
  <link rel="preload" as="image" href="/hero.webp" fetchpriority="high">
  ```
- **Don't lazy-load above-the-fold images** (only lazy-load images below the scroll)
- Use a CDN for faster delivery

---

## 2. INP — Interaction to Next Paint

### What is it?
**INP = when you click/tap, how long until the page reacts visually.**

Replaced the old "FID" metric in March 2024. Measures the *worst* delay across all interactions on the page, not just the first.

### Why it matters
Nothing feels worse than tapping a button and having the page freeze for 1 second. INP catches this.

### Common causes of bad INP
- Heavy JavaScript running on the main thread
- Big React re-renders blocking the UI
- Third-party scripts (analytics, ads, chat widgets)
- Slow event handlers doing too much work synchronously

### How to fix it
- Split heavy work using `setTimeout`, `requestIdleCallback`, or **web workers**
- Reduce JS bundle size (see `03-bundle-analysis`)
- Memoize expensive React components (`React.memo`, `useMemo`)
- Debounce input handlers
- Defer/async non-critical third-party scripts:
  ```html
  <script src="analytics.js" defer></script>
  ```

---

## 3. CLS — Cumulative Layout Shift

### What is it?
**CLS = how much stuff jumps around while the page is loading.**

You know when you're about to tap a button, then an ad loads above it and you accidentally tap the ad? That's a layout shift. CLS measures the total amount of unexpected movement.

### Why it matters
Layout shifts are infuriating. They cause misclicks and lost reading position.

### Common causes of bad CLS
- Images without `width` and `height` attributes
- Web fonts swapping in and changing text size (FOUT)
- Ads/embeds injected into the middle of content
- Animations that change layout (use `transform` instead)

### How to fix it
- **Always set `width` and `height` on images:**
  ```html
  <img src="/photo.jpg" width="800" height="600" alt="..." />
  ```
  This reserves the space *before* the image loads.
- Use `aspect-ratio` in CSS for responsive media:
  ```css
  img { aspect-ratio: 16/9; width: 100%; height: auto; }
  ```
- Preload web fonts and use `font-display: optional` or `swap`
- Reserve space for ads/embeds with a fixed-height container
- Use CSS `transform` for animations, not `top`/`left`/`width`

---

## 4. TTFB — Time to First Byte

### What is it?
**TTFB = how long the browser waits for the first byte of HTML from your server.**

If your server takes 2 seconds to respond, *everything* else is delayed by 2 seconds.

### How to fix it
- Use a CDN (Cloudflare, Vercel, Netlify all do this automatically)
- Use static generation (SSG) or ISR instead of server-rendering on every request
- Cache database queries
- Choose a fast hosting provider — avoid cheap shared hosting

---

## How to check Core Web Vitals

### Option 1: PageSpeed Insights (recommended)
1. Go to <https://pagespeed.web.dev/>
2. Paste your URL
3. See both **lab data** (synthetic test) and **field data** (real Chrome users)

### Option 2: Chrome DevTools
1. Open your site in Chrome
2. F12 → **Performance** tab
3. Click record, reload the page, stop recording
4. Look for "Web Vitals" annotations on the timeline

### Option 3: Web Vitals Chrome extension
Install: <https://chrome.google.com/webstore/detail/web-vitals/ahfhijdlegdabablpippeagghigmibma>
Live overlay showing LCP/INP/CLS as you browse your site.

### Option 4: In code (for real-user monitoring)
Install the `web-vitals` library:
```bash
npm install web-vitals
```
```js
import { onLCP, onINP, onCLS } from 'web-vitals';
onLCP(console.log);
onINP(console.log);
onCLS(console.log);
```

---

## ✅ Beginner action steps

1. Run PageSpeed Insights on your site right now.
2. Note your LCP, INP, CLS scores.
3. If LCP is bad → compress your hero image.
4. If CLS is bad → add `width`/`height` to all images.
5. Re-run the test. Celebrate improvements 🎉

**Next:** Read `02-lighthouse/README.md` to understand the full audit tool.
