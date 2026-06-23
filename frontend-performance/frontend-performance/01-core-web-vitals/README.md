# 📊 Core Web Vitals — What Are They?

> **Beginner summary:** Google measures 4 things about your website. These 4 things directly affect your Google ranking AND your users' experience.

---

## 🤔 What Are Core Web Vitals?

Core Web Vitals are **official metrics from Google** that measure how good or bad a website *feels* to use.

Think of it like a health checkup for your website. Just like a doctor checks your blood pressure, heart rate, and temperature — Google checks LCP, INP, and CLS.

**Why do they matter for YOU (a beginner)?**
- If your portfolio scores poorly → Google shows it less in search results
- If your portfolio scores poorly → Visitors leave before reading it
- If your portfolio scores poorly → You look less professional to recruiters

---

## 📐 The 4 Core Web Vitals (2025)

### 1. ⏱️ LCP — Largest Contentful Paint
**"How fast does the main content appear?"**

When you open a website, something big appears first — usually a hero image, a heading, or a big text block. LCP measures how long that takes.

→ See `LCP.md` for details

### 2. 👆 INP — Interaction to Next Paint
**"How fast does the page respond when I click?"**

When a user clicks a button, taps a link, or types in a form — how long until something visibly happens? INP measures that delay.

→ See `INP.md` for details

### 3. 📐 CLS — Cumulative Layout Shift
**"Does the page jump around while loading?"**

Ever clicked a button but missed because the page shifted at the last second? That's a layout shift. CLS measures how much this happens.

→ See `CLS.md` for details

### 4. 🌐 TTFB — Time to First Byte
**"How fast does the server respond?"**

When a browser requests your page, how long until the server sends back the first byte of data? This is about server speed, not browser speed.

→ See `TTFB.md` for details

---

## 🎯 Score Targets (Memorize These)

| Metric | 🟢 Good | 🟡 Needs Work | 🔴 Poor |
|--------|---------|--------------|---------|
| LCP | ≤ 2.5s | 2.5–4.0s | > 4.0s |
| INP | ≤ 200ms | 200–500ms | > 500ms |
| CLS | ≤ 0.1 | 0.1–0.25 | > 0.25 |
| TTFB | ≤ 800ms | 800ms–1.8s | > 1.8s |

---

## 🔍 How Do I Check My Website's Core Web Vitals?

### Method 1: Google PageSpeed Insights (Easiest — No Setup)
1. Go to **https://pagespeed.web.dev**
2. Paste your website URL (e.g. `https://myportfolio.com`)
3. Click **Analyze**
4. You'll see all 4 metrics with color-coded scores

### Method 2: Chrome DevTools (While Developing)
1. Open your website in Chrome
2. Right-click → **Inspect**
3. Click the **Lighthouse** tab
4. Click **Analyze page load**

### Method 3: Chrome Extension
- Install **Web Vitals** extension by Google (free)
- It shows a live badge on every website you visit

---

## 📈 How Google Uses These

Google uses a concept called **"passing the Core Web Vitals assessment"** — which means at least 75% of real user visits to your site hit the "Good" threshold for all 3 main metrics (LCP, INP, CLS).

If you pass → Google may boost your ranking
If you fail → Your site may rank lower than equally-good content

---

*Next: Read `LCP.md` to understand the most important metric →*
