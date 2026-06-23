# 02 — Lighthouse: Auditing Your Website

**Lighthouse** is a free, open-source tool built by Google that audits your website and gives you scores out of 100 across 5 categories:

1. **Performance** — is it fast?
2. **Accessibility** — can disabled users use it?
3. **Best Practices** — are you following web standards?
4. **SEO** — can Google find and rank it?
5. **PWA** (optional) — does it work like a native app?

It's the single most useful tool a beginner can learn.

---

## How to run Lighthouse (3 ways)

### Way 1: Chrome DevTools (easiest)
1. Open your site in **Google Chrome**
2. Press `F12` to open DevTools
3. Click the **Lighthouse** tab
4. Select categories: Performance, Accessibility, Best Practices, SEO
5. Select device: **Mobile** (Google ranks based on mobile!)
6. Click **Analyze page load**
7. Wait ~30 seconds → get your report

> 💡 Run it in an **Incognito window** to avoid Chrome extensions polluting results.

### Way 2: PageSpeed Insights (online, no install)
- Visit <https://pagespeed.web.dev/>
- Paste URL → click Analyze
- Bonus: shows you **real-user data** from Chrome users, not just lab tests

### Way 3: CLI (for developers)
```bash
npm install -g lighthouse
lighthouse https://yoursite.com --view
```
Generates an HTML report and opens it in your browser.

### Way 4: CI/CD (automated)
Use **Lighthouse CI** to fail builds if performance drops:
```bash
npm install -g @lhci/cli
lhci autorun
```
Great for teams — catches regressions before they ship.

---

## Understanding your scores

| Score | Grade |
|-------|-------|
| 90–100 | 🟢 Good |
| 50–89 | 🟠 Needs improvement |
| 0–49 | 🔴 Poor |

**Don't obsess over 100/100.** A score of 90+ on mobile is excellent. The last 10 points often cost more effort than they're worth.

---

## The 5 categories explained

### 🚀 Performance
Measures load speed using metrics like LCP, INP, CLS, TBT (Total Blocking Time), Speed Index.

**Common fixes Lighthouse suggests:**
- "Properly size images" → resize huge images
- "Serve images in next-gen formats" → use WebP/AVIF
- "Eliminate render-blocking resources" → defer non-critical CSS/JS
- "Reduce unused JavaScript" → tree-shake or code-split
- "Preconnect to required origins" → `<link rel="preconnect" href="...">`

See `01-core-web-vitals/` and `03-bundle-analysis/` for fixes.

---

### ♿ Accessibility
Can people with disabilities (screen readers, motor impairments, color blindness) use your site?

**Common fixes:**
- Add `alt` text to every image:
  ```html
  <img src="/cat.jpg" alt="An orange tabby cat sleeping" />
  ```
- Use semantic HTML: `<header>`, `<nav>`, `<main>`, `<button>` (not `<div onclick>`)
- Ensure color contrast is at least 4.5:1 (text vs background)
- Add labels to form inputs:
  ```html
  <label for="email">Email</label>
  <input id="email" type="email" />
  ```
- Make sure interactive elements are keyboard-accessible (Tab key works)

> Accessibility = SEO + usability + legal compliance. Always fix these.

---

### ✅ Best Practices
Generic web hygiene checks.

**Common fixes:**
- Use **HTTPS** everywhere
- Don't use deprecated APIs
- Avoid `document.write()`
- Set a correct `<!DOCTYPE html>`
- No console errors in production
- Images have correct aspect ratios

---

### 🔍 SEO
Can Google index and rank your site?

**Common fixes:**
- Add a `<title>` tag (under 60 characters)
- Add a `<meta name="description">` (under 160 chars)
- Add a `viewport` meta tag (mobile-friendly):
  ```html
  <meta name="viewport" content="width=device-width, initial-scale=1">
  ```
- Use one `<h1>` per page
- Make links descriptive (`Read our pricing` not `Click here`)
- Add a `robots.txt` and `sitemap.xml`
- Use `rel="canonical"` to avoid duplicate content

---

### 📱 PWA (Progressive Web App)
Optional. Only relevant if you want your site to behave like an installable app.

Requires:
- A `manifest.json`
- A service worker
- HTTPS
- Offline support

For a portfolio site, **skip this**.

---

## Reading the report — what to focus on

After running Lighthouse, you'll see:

1. **Scores** at the top (out of 100)
2. **Metrics** (LCP, INP, CLS, etc.) with their actual values
3. **Opportunities** — fixes that will speed up load time, sorted by impact
4. **Diagnostics** — deeper issues
5. **Passed audits** — what you already did right

**Focus on Opportunities first** — they're sorted by estimated time savings.

---

## ✅ Beginner action steps

1. Run Lighthouse on your site in **mobile mode**.
2. Screenshot the scores so you can compare later.
3. Pick the **top 3 Opportunities** and fix them.
4. Re-run Lighthouse. Compare. Repeat.
5. Aim for **all 4 categories above 90**.

> 💡 Pro tip: Run Lighthouse on your favorite websites (Apple, Stripe, Vercel). See how the pros do it.

**Next:** `03-bundle-analysis/README.md` — learn how to make your JavaScript smaller.
