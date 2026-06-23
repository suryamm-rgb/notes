# 📊 How to Measure Your Website Performance Score Today

> Step-by-step guide. Takes 10 minutes. No setup required.

---

## Step 1: Get Your Baseline Score

1. Go to **https://pagespeed.web.dev**
2. Enter your website URL
3. Click **Analyze**
4. Wait ~30 seconds
5. Screenshot the results (you will compare these later)

**Write down:**
- Performance score: ____
- Accessibility score: ____
- Best Practices score: ____
- SEO score: ____
- LCP: ____
- INP: ____
- CLS: ____

---

## Step 2: Test on Mobile (More Important)

On the same PageSpeed Insights page, click the **Mobile** tab.

Mobile scores are almost always lower because Google simulates a slower CPU and network. This is what Google uses for ranking.

---

## Step 3: Run Lighthouse in Chrome

For your locally-running development site:

1. Run your project: `npm run dev`
2. Open `http://localhost:3000` in Chrome Incognito
3. Press F12 → Lighthouse tab
4. Select Mobile → Analyze page load

---

## Step 4: Find Your Biggest Issues

In the Lighthouse report, scroll to **Opportunities**. Sort by largest savings.

The TOP item is your first fix. Fix it, then re-run Lighthouse.

---

## Step 5: Fix the Most Common Issues

### If Performance < 90:
1. Check image sizes — are any images > 200KB?
2. Run `npm run build` and check "First Load JS" column
3. Check if hero image has `priority` prop

### If Accessibility < 100:
1. Check all images have `alt` text
2. Check all buttons have labels
3. Run axe DevTools extension for detailed findings

### If SEO < 100:
1. Add `meta description` to every page
2. Ensure page has a unique `<title>`
3. Check no links use `href="javascript:void(0)"`

### If Best Practices < 90:
1. Open DevTools Console (F12) — fix any red errors
2. Ensure site uses HTTPS

---

## Step 6: Re-measure and Compare

After each fix, re-run PageSpeed Insights and compare to your baseline.

Track progress in a table:

| Date | Performance | Accessibility | SEO | LCP | CLS |
|------|------------|--------------|-----|-----|-----|
| Day 1 | 52 | 79 | 82 | 4.1s | 0.31 |
| Day 2 | 78 | 100 | 100 | 2.8s | 0.09 |
| Day 7 | 94 | 100 | 100 | 1.6s | 0.02 |

---

## Goal Scores for a Portfolio

| Category | Minimum | Target |
|----------|---------|--------|
| Performance | 85 | 95+ |
| Accessibility | 95 | 100 |
| Best Practices | 90 | 100 |
| SEO | 95 | 100 |
