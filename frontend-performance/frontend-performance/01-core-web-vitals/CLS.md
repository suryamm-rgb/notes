# 📐 CLS — Cumulative Layout Shift

> **In plain English:** Does your page jump around while loading? CLS measures how much.

---

## What Is CLS?

CLS measures the **visual stability** of your page.

A "layout shift" happens when a visible element moves from its expected position — for example, when an image loads and pushes all the text down, or when a font swap changes text size and shifts everything below it.

**Real-world example of a bad CLS:**
1. You open a news article
2. You start reading the headline
3. An ad banner loads above it — the headline jumps down 200px
4. You accidentally click the wrong thing
5. That's a layout shift!

---

## 🎯 CLS Targets

| Score | Value |
|-------|-------|
| 🟢 Good | ≤ 0.1 |
| 🟡 Needs Improvement | 0.1 – 0.25 |
| 🔴 Poor | > 0.25 |

> CLS is a **score**, not a time. 0 means no shifting at all. Higher = more shifting.

---

## ❌ Common Causes of Bad CLS

### Problem 1: Images without width/height (THE #1 cause)

When a browser doesn't know an image's size, it allocates 0px of space. When the image loads, it pushes everything down.

```html
<!-- ❌ BAD: No dimensions — browser has no idea how much space to reserve -->
<img src="/hero.jpg" alt="Hero" />

<!-- ✅ GOOD: Explicit dimensions — browser reserves space before image loads -->
<img src="/hero.jpg" alt="Hero" width="1200" height="600" />
```

```jsx
// ✅ BEST (Next.js): next/image handles this automatically
import Image from 'next/image'

<Image
  src="/hero.jpg"
  alt="Hero"
  width={1200}
  height={600}
/>
```

### Problem 2: Dynamically injected content (ads, banners, cookie notices)

```css
/* ✅ GOOD: Reserve space for ads before they load */
.ad-container {
  min-height: 90px;  /* Reserve the space */
  width: 100%;
}
```

### Problem 3: Web fonts causing text shift (FOUT)

When a web font loads, it can cause text to reflow if the fallback font has different dimensions.

```css
/* ✅ GOOD: Use size-adjust to match fallback font metrics */
@font-face {
  font-family: 'MyFont';
  src: url('/fonts/myfont.woff2') format('woff2');
  font-display: swap;
  size-adjust: 105%;  /* Adjust to match fallback font size */
}
```

### Problem 4: CSS animations that trigger layout

```css
/* ❌ BAD: Animating width/height/margin causes layout recalculation */
.box {
  transition: width 0.3s ease;
}

/* ✅ GOOD: Animate transform/opacity — these don't cause layout shifts */
.box {
  transition: transform 0.3s ease;
}
.box:hover {
  transform: scale(1.05);  /* No layout shift! */
}
```

### Problem 5: Content injected above existing content

```javascript
// ❌ BAD: Notification banner inserts ABOVE existing content
document.body.insertBefore(notificationBanner, document.body.firstChild)

// ✅ GOOD: Fixed position or reserved space at top
// In CSS: position: fixed; or pre-allocate space with a placeholder
```

---

## ✅ CLS Checklist for Portfolio Sites

- [ ] All `<img>` tags have `width` and `height` attributes
- [ ] Using `next/image` (handles dimensions automatically)
- [ ] Cookie/GDPR banners use `position: fixed` (don't push content)
- [ ] No ads or banners injected above existing content
- [ ] Animations use `transform` and `opacity`, not `width`/`height`/`top`/`left`
- [ ] Skeleton loaders used for dynamic content areas
- [ ] Font loading doesn't significantly change text layout

---

## 🛠️ How to Debug CLS

```javascript
// Paste in DevTools console to see what's shifting
new PerformanceObserver((list) => {
  for (const entry of list.getEntries()) {
    if (!entry.hadRecentInput) {
      console.log('Layout shift:', entry.value, entry.sources)
    }
  }
}).observe({ type: 'layout-shift', buffered: true })
```

This logs every layout shift with the **element** that caused it.

---

*Next: Read `TTFB.md` →*
