# 🔦 Lighthouse — Your Website's Health Scanner

> **In plain English:** Lighthouse is a free tool built into Chrome that gives your website a score out of 100 and tells you exactly what to fix.

---

## What Is Lighthouse?

Lighthouse is an **open-source, automated tool** from Google that audits websites across 5 categories:

| Category | What It Checks |
|----------|---------------|
| ⚡ **Performance** | How fast your page loads |
| ♿ **Accessibility** | Can everyone (including disabled users) use your site? |
| ✅ **Best Practices** | Is your code modern and secure? |
| 🔍 **SEO** | Can Google find and understand your content? |
| 📱 **PWA** | Is it a Progressive Web App? (optional) |

Each category gets a score from **0–100**:
- 🟢 90–100 = Good
- 🟡 50–89 = Needs Improvement  
- 🔴 0–49 = Poor

---

## 🚀 Quick Start: Run Lighthouse in 60 Seconds

### Method 1: Chrome DevTools (No Install Required)

1. Open your website in **Google Chrome**
2. Press `F12` (Windows/Linux) or `Cmd+Option+I` (Mac) to open DevTools
3. Click the **"Lighthouse"** tab (may be hidden under `>>`)
4. Select categories: ✅ Performance ✅ Accessibility ✅ Best Practices ✅ SEO
5. Select mode: **Navigation (Default)**
6. Select device: **Mobile** (test mobile first — it's harder!)
7. Click **"Analyze page load"**
8. Wait ~30 seconds for results

→ See `HOW-TO-RUN-LIGHTHOUSE.md` for detailed walkthrough

---

## What Each Score Means for Your Portfolio

### Performance Score
- Affects how fast your portfolio loads
- **Target:** 90+ (mobile) / 95+ (desktop)

### Accessibility Score
- Affects whether ALL users can use your site
- **Target:** 100 (zero tolerance — it's about people)
- Even if you don't care about SEO, accessibility matters morally

### Best Practices Score
- Affects security, modern standards
- **Target:** 95+

### SEO Score
- Affects whether Google can find and rank your portfolio
- **Target:** 100 (it's mostly easy wins)

---

## 📂 Files in This Section

| File | What You'll Learn |
|------|-------------------|
| `HOW-TO-RUN-LIGHTHOUSE.md` | Step-by-step first audit |
| `PERFORMANCE-AUDITS.md` | Fix every performance issue |
| `ACCESSIBILITY.md` | Fix accessibility warnings |
| `SEO-AUDITS.md` | Fix SEO issues |
| `BEST-PRACTICES.md` | Fix best practices warnings |

---

## ⚠️ Important: Run in Incognito Mode

Browser extensions can interfere with Lighthouse results and give you inaccurate scores.

Always run Lighthouse in **Chrome Incognito window** for accurate results:
- Windows: `Ctrl+Shift+N`
- Mac: `Cmd+Shift+N`

---

*Start with: `HOW-TO-RUN-LIGHTHOUSE.md` →*
