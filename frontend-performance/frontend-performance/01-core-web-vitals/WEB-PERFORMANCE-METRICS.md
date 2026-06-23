# 📈 Other Web Performance Metrics

> Beyond Core Web Vitals, there are several other metrics worth knowing about.

---

## FCP — First Contentful Paint

**"When does ANYTHING appear on screen?"**

FCP measures when the browser renders the first bit of content (text, image, SVG, non-white canvas).

| 🟢 Good | 🟡 Needs Work | 🔴 Poor |
|---------|--------------|---------|
| ≤ 1.8s | 1.8s – 3.0s | > 3.0s |

**FCP vs LCP:** FCP is the first ANYTHING. LCP is the largest thing. You want both fast, but LCP is more important.

**Common Fixes:**
- Remove render-blocking resources (scripts/styles in `<head>`)
- Reduce server response time
- Use `font-display: swap` for web fonts

---

## TBT — Total Blocking Time

**"How long was the main thread blocked?"**

TBT measures how much time the browser's main thread was "blocked" — meaning it couldn't respond to user input.

| 🟢 Good | 🟡 Needs Work | 🔴 Poor |
|---------|--------------|---------|
| ≤ 200ms | 200–600ms | > 600ms |

**What causes high TBT?**
- Large JavaScript bundles that take long to parse
- Heavy third-party scripts (chat widgets, analytics)
- Expensive JavaScript executing on load

**Common Fix:**
```javascript
// Defer non-critical scripts
<script src="analytics.js" defer></script>
<script src="chat-widget.js" defer></script>
```

---

## Speed Index

**"How quickly does the page visually fill in?"**

Speed Index measures how quickly content is visually displayed during page load. It's a composite score, not a single moment.

| 🟢 Good | 🟡 Needs Work | 🔴 Poor |
|---------|--------------|---------|
| ≤ 3.4s | 3.4s – 5.8s | > 5.8s |

---

## TTI — Time to Interactive

**"When can users actually use the page?"**

TTI measures when the page becomes **fully interactive** — meaning:
- The page has displayed useful content
- Event handlers are registered for visible elements
- The page responds to interactions within 50ms

> **Note:** TTI has become less emphasized in 2024+ as INP is more actionable. Lighthouse still reports it.

| 🟢 Good | 🟡 Needs Work | 🔴 Poor |
|---------|--------------|---------|
| ≤ 3.8s | 3.8s – 7.3s | > 7.3s |

---

## FID — First Input Delay (Deprecated)

FID was replaced by **INP** in March 2024. It measured the delay on the first interaction only.

If you see FID in old articles/docs, think "that's now INP."

---

## Summary Table: All Metrics

| Metric | Measures | Good Target | Still Active? |
|--------|---------|-------------|---------------|
| **LCP** | Main content speed | ≤ 2.5s | ✅ Core Web Vital |
| **INP** | Interaction speed | ≤ 200ms | ✅ Core Web Vital |
| **CLS** | Layout stability | ≤ 0.1 | ✅ Core Web Vital |
| **TTFB** | Server speed | ≤ 800ms | ✅ Core Web Vital |
| **FCP** | First paint | ≤ 1.8s | ✅ Diagnostic |
| **TBT** | JS blocking time | ≤ 200ms | ✅ Diagnostic |
| **Speed Index** | Visual fill speed | ≤ 3.4s | ✅ Lighthouse |
| **TTI** | Full interactivity | ≤ 3.8s | ⚠️ Lighthouse only |
| **FID** | First input delay | ≤ 100ms | ❌ Deprecated |

---

## 🎯 Priority Order for Fixing

1. **LCP** — Biggest impact on perceived speed
2. **CLS** — Biggest impact on usability  
3. **INP** — Biggest impact on interactivity
4. **TTFB** — Foundation everything else depends on
5. **FCP** — Often fixed alongside LCP
6. **TBT** — Helps INP and TTI

---

*Next: Go to `02-lighthouse/HOW-TO-RUN-LIGHTHOUSE.md` to measure your site →*
