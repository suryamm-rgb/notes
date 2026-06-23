# 🔦 How to Run Your First Lighthouse Audit

> **Complete beginner guide.** Follow every step exactly.

---

## Before You Start

You need:
- ✅ Google Chrome browser (free: https://www.google.com/chrome)
- ✅ Your website URL (can be live site or localhost)

---

## Method 1: Audit a Live Website (Recommended)

### Step 1: Open Chrome Incognito
Press `Ctrl+Shift+N` (Windows) or `Cmd+Shift+N` (Mac)

*Why incognito? Extensions can affect scores and give false results.*

### Step 2: Navigate to Your Site
Type your URL in the address bar and press Enter.

Example: `https://myportfolio.vercel.app`

### Step 3: Open DevTools
- Windows/Linux: Press `F12`
- Mac: Press `Cmd + Option + I`

### Step 4: Find the Lighthouse Tab
Look at the tabs across the top of DevTools:
`Elements | Console | Sources | Network | Performance | ... >>`

Lighthouse might be hidden. Click `>>` to find it.

### Step 5: Configure Your Audit

Set these options:
```
Mode:       Navigation (Default)  ← Audits a page load
Device:     Mobile                ← Test mobile first (harder = more useful)
Categories: ✅ Performance
            ✅ Accessibility
            ✅ Best practices
            ✅ SEO
            (uncheck PWA — not needed for portfolios)
```

### Step 6: Click "Analyze page load"

Wait 30–60 seconds. Don't click anything. Chrome will:
1. Reload your page multiple times
2. Simulate a slow network & CPU
3. Measure everything
4. Generate your report

### Step 7: Read Your Report

You'll see 4 circular score gauges. Example:

```
Performance: 72  ← Needs work
Accessibility: 88  ← Close!
Best Practices: 100  ← 
SEO: 91  ← Good
```

**Scroll down** to see:
- **Metrics** — the actual Core Web Vitals numbers
- **Opportunities** — things you CAN fix to improve score
- **Diagnostics** — extra info about issues
- **Passed audits** — things already working ✅

---

## Method 2: PageSpeed Insights (No Browser Needed)

This uses **real user data** from the Chrome User Experience Report.

1. Go to **https://pagespeed.web.dev**
2. Enter your URL
3. Click **Analyze**
4. See both **Mobile** and **Desktop** results

**Advantage over DevTools Lighthouse:** Shows real-world data, not just lab data.

---

## Method 3: Audit Localhost (During Development)

If your site is running locally (`http://localhost:3000`):

```bash
# Install Lighthouse CLI globally
npm install -g lighthouse

# Run audit on your local site
lighthouse http://localhost:3000 --view

# Run and save report to a file
lighthouse http://localhost:3000 --output html --output-path ./lighthouse-report.html
```

This opens a full HTML report in your browser.

---

## Understanding the Report: Opportunities vs Diagnostics

### Opportunities (Fix These First!)
These have an **estimated savings time** next to them.

Example:
```
Opportunity                          Estimated Savings
----------------------------------------
Serve images in next-gen formats     → 1.2s
Eliminate render-blocking resources  → 0.8s
Properly size images                 → 0.5s
```

Start with the **biggest savings** first.

### Diagnostics
These don't have direct savings estimates but still matter:
```
Diagnostic
-----------
Image elements do not have explicit width and height
Ensure text remains visible during webfont load
Links do not have descriptive text
```

---

## 🔁 The Improvement Loop

```
1. Run Lighthouse → Note your score
2. Fix ONE issue (start with biggest savings)
3. Run Lighthouse again → See improvement
4. Repeat until all scores are 90+
```

Don't try to fix everything at once. Fix one thing, measure, repeat.

---

## 📊 Example: Portfolio Starting Scores vs. After Optimization

| Category | Before | After 1 Week |
|----------|--------|-------------|
| Performance | 52 🔴 | 94 🟢 |
| Accessibility | 79 🟡 | 100 🟢 |
| Best Practices | 83 🟡 | 100 🟢 |
| SEO | 82 🟡 | 100 🟢 |

**What changed?**
- Compressed hero image from 2.4MB → 180KB
- Added `alt` text to all images
- Added missing `meta description`
- Fixed missing `width`/`height` on images
- Deferred analytics script

→ See `PERFORMANCE-AUDITS.md` for how to fix each issue
