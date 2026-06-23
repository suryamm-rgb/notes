# ✅ Portfolio Performance Checklist

> Use this before launching your portfolio or after every major update.

---

## 🖼️ Images

- [ ] All images use `next/image` (not plain `<img>`)
- [ ] Hero/above-fold image has `priority` prop
- [ ] All images have descriptive `alt` text
- [ ] All images are WebP format (converted via Squoosh)
- [ ] Hero image < 200KB
- [ ] Project screenshots < 150KB each
- [ ] Profile photo < 60KB
- [ ] All `<img>` and `<Image>` have explicit `width` and `height`
- [ ] SVG used for icons and logos instead of PNG

---

## ⚡ JavaScript

- [ ] `npm run build` shows all routes < 150KB First Load JS
- [ ] Heavy libraries loaded with `dynamic()` or not at all
- [ ] No moment.js (use date-fns instead)
- [ ] Icon imports are specific (not `import * as Icons from 'react-icons'`)
- [ ] `console.log` statements removed from production code

---

## 📐 Layout Stability (CLS)

- [ ] No layout shifts on page load (check with Chrome DevTools)
- [ ] Cookie banner (if any) uses `position: fixed`
- [ ] No content injected above existing content on load
- [ ] Fonts use `font-display: swap`
- [ ] Animation uses `transform`/`opacity` not `width`/`height`

---

## ⚛️ React Performance

- [ ] All list items have unique, stable `key` props
- [ ] No inline object/function props on frequently-rendered components
- [ ] Contact form / heavy components use dynamic imports

---

## 🔍 SEO

- [ ] Each page has a unique `<title>`
- [ ] Each page has a unique `meta description` (150–160 chars)
- [ ] `robots.txt` exists in `/public`
- [ ] Sitemap configured (Next.js `app/sitemap.js`)
- [ ] Open Graph image configured (1200x630px)
- [ ] All links use real `href` values (no `javascript:void(0)`)
- [ ] `<html lang="en">` attribute set

---

## ♿ Accessibility

- [ ] All images have `alt` text
- [ ] All icon buttons have `aria-label`
- [ ] All links have descriptive text (not just "click here")
- [ ] Color contrast passes 4.5:1 for body text
- [ ] All form inputs have associated labels
- [ ] Heading order is logical (h1 → h2 → h3)
- [ ] Keyboard navigation works (tab through all interactive elements)
- [ ] Lighthouse Accessibility score = 100

---

## 🌐 Next.js

- [ ] Portfolio pages use Static Generation (no `dynamic = 'force-dynamic'` unless needed)
- [ ] Static pages do not use `cache: 'no-store'` unnecessarily
- [ ] Server Components used for data fetching (not Client Components)
- [ ] `"use client"` only on components that need interactivity

---

## 🚀 Deployment

- [ ] Deployed to Vercel, Netlify, or Cloudflare Pages
- [ ] HTTPS enabled
- [ ] Custom domain configured (optional but professional)
- [ ] Environment variables set correctly (not hardcoded)

---

## 📊 Final Scores (Run Before Launch)

Run https://pagespeed.web.dev on your live URL and confirm:

| Metric | Your Score | Target |
|--------|-----------|--------|
| Performance | | 90+ |
| Accessibility | | 100 |
| Best Practices | | 95+ |
| SEO | | 100 |
| LCP | | < 2.5s |
| CLS | | < 0.1 |
| INP | | < 200ms |

---

*If all boxes are checked and scores hit targets — your portfolio is production-ready.*
