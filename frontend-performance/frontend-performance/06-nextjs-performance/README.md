# ⚡ Next.js Performance — Rendering Strategies

> Next.js gives you multiple ways to render pages. Choosing the right one is the most impactful performance decision you can make.

---

## The 4 Rendering Strategies

| Strategy | When HTML is Built | Speed | Use For |
|----------|--------------------|-------|---------|
| **Static Generation (SSG)** | At build time | Fastest | Portfolio, blogs, docs |
| **ISR** | At build time + background refresh | Fast | Content that updates occasionally |
| **SSR** | On every request | Slower | User-specific content, real-time data |
| **Client-Side** | In the browser | Varies | Dashboards, interactive apps |

---

## Which Should YOU Use for a Portfolio?

```
Portfolio homepage       → Static Generation (SSG)
Projects page            → Static Generation (SSG)
Blog posts               → Static Generation (SSG) or ISR
Contact form             → Static page + API route for form submission
Dashboard (if any)       → SSR or Client-side
```

**For a portfolio: 99% Static Generation.** This gives you the fastest possible site.

---

## Files in This Section

| File | Topic |
|------|-------|
| `SERVER-COMPONENTS.md` | React Server Components (new in Next.js 13+) |
| `CLIENT-COMPONENTS.md` | When to add "use client" |
| `CACHING.md` | How Next.js caches things |
| `ISR.md` | Incremental Static Regeneration |
| `SSR.md` | Server-Side Rendering |
| `STATIC-GENERATION.md` | Static Site Generation (fastest) |

---

## Quick Decision Tree

```
Does the page need user-specific data? (logged-in user, personalization)
  YES → SSR or Client-side rendering
  NO  →
    Does the data update frequently? (stock prices, live scores)
      YES → SSR
      NO  →
        Does the data update occasionally? (blog posts, CMS content)
          YES → ISR
          NO  → Static Generation (SSG)  ← Most portfolio pages
```
