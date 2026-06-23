# Frontend Performance Optimization — Beginner's Guide

Welcome! This guide is written for someone who is **brand new** to web performance. If you just built your first portfolio or simple website and you're wondering:

- "Is my site fast?"
- "How do I even check that?"
- "Why does performance matter?"
- "What can I do to make it faster?"

…then you're in the right place.

---

## 🤔 Why does performance matter?

Imagine you open a website on your phone and it takes 6 seconds to show anything. What do you do? You close it. **53% of users leave a mobile site if it takes more than 3 seconds to load** (Google research).

Performance affects:

| Area | Impact |
|------|--------|
| **User experience** | Fast sites feel professional, slow sites feel broken |
| **SEO (Google ranking)** | Google ranks fast sites higher in search results |
| **Conversions** | Every 100ms delay = ~1% drop in sales (Amazon study) |
| **Accessibility** | Slow sites are unusable on cheap phones / slow networks |
| **Bounce rate** | Slow sites lose visitors immediately |

For a **portfolio site**, performance signals professionalism. A recruiter who waits 5 seconds for your hero image to load will already have a bad impression.

---

## 📚 How to use this guide

Read the folders **in order**. Each folder builds on the previous one.

```
frontend-performance/
├── README.md                          ← you are here
├── 01-core-web-vitals/                ← the metrics Google cares about
├── 02-lighthouse/                     ← the tool that scores your site
├── 03-bundle-analysis/                ← making your JavaScript smaller
├── 04-image-optimization/             ← images are usually the biggest problem
├── 05-react-performance/              ← if you use React
└── 06-nextjs-performance/             ← if you use Next.js
```

Inside each folder there's a `README.md` that explains:
1. **What** the topic is (in plain English)
2. **Why** it matters
3. **How** to check / measure it
4. **How** to fix or improve it
5. **Beginner action steps** you can do today

---

## 🚀 Quickstart: "I just want to check my site NOW"

If you only have 5 minutes, do this:

### Step 1 — Open Chrome
Open your website in **Google Chrome** (desktop).

### Step 2 — Open DevTools
Press `F12` (or `Cmd+Option+I` on Mac).

### Step 3 — Run Lighthouse
1. Click the **Lighthouse** tab in DevTools
2. Select: **Performance**, **Accessibility**, **Best Practices**, **SEO**
3. Choose **Mobile** (this is what Google scores you on)
4. Click **Analyze page load**

You'll get 4 scores out of 100. Anything below 90 has room to improve.

### Step 4 — Run PageSpeed Insights (real-world data)
Go to: <https://pagespeed.web.dev/>

Paste your URL. This uses the same Lighthouse engine but also shows **real user data** from Google Chrome users.

### Step 5 — Read the report
The report tells you exactly what's slow and how to fix it. Then come back to this guide and read the matching folder.

---

## 🎯 The 3 biggest wins for beginners

If you do nothing else, do these:

1. **Compress your images** → see `04-image-optimization/`
2. **Don't load fonts/scripts you don't use** → see `03-bundle-analysis/`
3. **Set width/height on images** so the page doesn't jump around → see `01-core-web-vitals/` (CLS section)

These three fixes alone can take a site from a 40 score to a 90+ score.

---

## 📖 Glossary (terms you'll see everywhere)

| Term | Plain English |
|------|---------------|
| **LCP** | How long until the biggest thing on screen appears |
| **INP** | How fast the page reacts when you click/tap |
| **CLS** | How much the page jumps around while loading |
| **TTFB** | How long until the server sends the first byte |
| **Bundle** | All your JS/CSS files smashed together for the browser |
| **Lazy load** | Don't load something until the user needs it |
| **Tree shaking** | Automatically remove code you don't use |
| **SSR** | Server-Side Rendering (HTML built on server) |
| **CSR** | Client-Side Rendering (HTML built in browser) |
| **ISR** | Incremental Static Regeneration (rebuild pages occasionally) |

---

## 🛠 Tools you'll use

| Tool | Purpose | Cost |
|------|---------|------|
| Chrome DevTools Lighthouse | Audit any site | Free |
| PageSpeed Insights | Audit + real user data | Free |
| WebPageTest.org | Deep waterfall analysis | Free |
| GTmetrix | Visual reports | Free tier |
| Bundle analyzer (Vite/Webpack/Next) | See what's in your JS | Free |
| Squoosh.app | Compress images in browser | Free |

---

## ✅ Recommended learning path

**Week 1:** Read `01-core-web-vitals` + `02-lighthouse`. Run Lighthouse on your own site.
**Week 2:** Read `04-image-optimization`. Compress every image on your site.
**Week 3:** Read `03-bundle-analysis`. Look at what's in your JS bundle.
**Week 4:** Read `05-react-performance` (if you use React) and `06-nextjs-performance` (if you use Next.js).

After 4 weeks you'll know more about web performance than 90% of working developers. 💪

---

Now jump into `01-core-web-vitals/README.md` to start learning. Good luck!
