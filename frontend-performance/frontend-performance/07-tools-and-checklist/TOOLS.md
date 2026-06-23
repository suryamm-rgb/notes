# 🛠️ Tools You Need — All Free

> Complete list of tools to measure and improve your website performance.

---

## Measurement Tools

### Google PageSpeed Insights
- **URL:** https://pagespeed.web.dev
- **What:** Tests your live site and shows Core Web Vitals, Lighthouse scores, real user data
- **When to use:** Before and after every optimization
- **Cost:** Free

### Chrome Lighthouse (Built into Chrome)
- **How:** Press F12 → Lighthouse tab
- **What:** Full audit including performance, accessibility, SEO, best practices
- **When to use:** While developing locally
- **Cost:** Free

### Google Search Console
- **URL:** https://search.google.com/search-console
- **What:** Shows real-world Core Web Vitals from actual Google Chrome users visiting your site
- **When to use:** After your site has been live for at least 28 days
- **Cost:** Free

### WebPageTest
- **URL:** https://www.webpagetest.org
- **What:** Detailed waterfall charts, tests from different locations/devices
- **When to use:** Deep debugging of load performance
- **Cost:** Free

### GTmetrix
- **URL:** https://gtmetrix.com
- **What:** Alternative to PageSpeed Insights, saves history
- **Cost:** Free (with account)

---

## Image Optimization Tools

### Squoosh
- **URL:** https://squoosh.app
- **What:** Convert images to WebP/AVIF, compare quality vs size
- **Best for:** One-off image optimization

### TinyPNG / TinyJPG
- **URL:** https://tinypng.com
- **What:** Quick PNG/JPEG compression

### SVGOMG
- **URL:** https://svgomg.net
- **What:** Optimize SVG files

### ImageOptim (Mac)
- **URL:** https://imageoptim.com
- **What:** Batch compress images on Mac

---

## Bundle Analysis

### Next.js Bundle Analyzer
```bash
npm install --save-dev @next/bundle-analyzer
```
See `03-bundle-analysis/NEXTJS-ANALYZER.md`

### Bundlephobia
- **URL:** https://bundlephobia.com
- **What:** Check the size of any npm package before installing it
- **Example:** Search "moment" — see it is 67KB! Then search "date-fns" — much smaller.

---

## Accessibility Tools

### axe DevTools (Chrome Extension)
- Install from Chrome Web Store: "axe DevTools"
- Finds accessibility issues beyond what Lighthouse reports

### WebAIM Contrast Checker
- **URL:** https://webaim.org/resources/contrastchecker/
- **What:** Check if text has enough contrast against background

### Screen Reader (Free)
- Mac: VoiceOver (built-in, Cmd+F5)
- Windows: NVDA (free, https://nvaccess.org)

---

## Development Extensions

### React Developer Tools
- Install from Chrome Web Store
- Adds a Profiler tab to DevTools for measuring React render times

### Web Vitals Extension (by Google)
- Install from Chrome Web Store: "Web Vitals"
- Shows LCP, INP, CLS badge on every page you visit

---

## Deployment Platforms (Free Tiers)

| Platform | Free Tier | Best For |
|----------|-----------|---------|
| **Vercel** | Generous | Next.js (made by same team) |
| **Netlify** | Generous | Any static site |
| **Cloudflare Pages** | Very generous | Maximum global speed |
| **GitHub Pages** | Free | Simple static sites |
