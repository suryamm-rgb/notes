# 🌐 TTFB — Time to First Byte

> **In plain English:** How long until the server starts sending your page back to the browser?

---

## What Is TTFB?

TTFB measures the time between a browser sending a request for a page and receiving the **first byte** of the response.

**The journey looks like this:**

```
User types URL → DNS Lookup → TCP Connection → Server processes request → First byte arrives
         ↑                                              ↑
    [starts here]                               [TTFB ends here]
```

Everything that happens on the server side affects TTFB.

---

## 🎯 TTFB Targets

| Score | Time |
|-------|------|
| 🟢 Good | ≤ 800ms |
| 🟡 Needs Improvement | 800ms – 1,800ms |
| 🔴 Poor | > 1,800ms |

---

## What Affects TTFB?

### 1. Hosting / Server Location
If your server is in the US and your visitor is in India, the data has to travel a long way.

**Fix:** Use a CDN (Content Delivery Network) — it caches your site closer to users worldwide.

### 2. Server Processing Time
If your server runs complex database queries before sending the page, TTFB increases.

**Fix:** Use Static Generation (SSG) or caching.

### 3. Rendering Strategy (Next.js)
| Strategy | TTFB | Why |
|----------|------|-----|
| Static (SSG) | Fastest 🟢 | HTML pre-built, served instantly |
| ISR | Fast 🟢 | Mostly pre-built, occasionally rebuilt |
| SSR | Slower 🟡 | HTML built on each request |
| Client-side only | Varies 🟡 | Fast server, slow content |

---

## ✅ How to Improve TTFB

### Option 1: Use Static Generation (Easiest for Portfolios)
```javascript
// pages/index.js (Next.js Pages Router)
export async function getStaticProps() {
  const projects = await fetchProjects()  // Runs at BUILD TIME, not per-request
  return { props: { projects } }
}

// app/page.js (Next.js App Router)
// By default, Server Components are statically rendered
export default async function Page() {
  const projects = await fetchProjects()  // Cached automatically
  return <ProjectList projects={projects} />
}
```

### Option 2: Add Caching Headers
```javascript
// Next.js: Add cache headers to API routes
export async function GET() {
  const data = await fetchData()
  return Response.json(data, {
    headers: {
      'Cache-Control': 'public, s-maxage=3600, stale-while-revalidate=86400'
      //                             ↑                      ↑
      //              Cache for 1 hour on CDN    Serve stale for 1 day while rebuilding
    }
  })
}
```

### Option 3: Deploy on a Fast Platform
For portfolio/personal sites, these platforms have excellent TTFB:

| Platform | Free Tier | TTFB |
|----------|-----------|------|
| **Vercel** | ✅ Generous | Very fast (global CDN) |
| **Netlify** | ✅ Generous | Fast |
| **Cloudflare Pages** | ✅ Very generous | Fastest (edge network) |
| GitHub Pages | ✅ Free | Decent |

---

## 📊 Checking TTFB

### In Chrome DevTools:
1. Open DevTools (`F12`)
2. Go to **Network** tab
3. Reload the page
4. Click the first request (your HTML file)
5. In the **Timing** section, look for **"Waiting for server response"** — that's your TTFB

### In PageSpeed Insights:
The "Server Response Time" audit directly measures TTFB.

---

## 💡 For Your Portfolio

Your portfolio is almost certainly **static content** (no database, no user accounts). This means you can achieve near-zero server processing time by using Static Generation + a CDN.

**Realistic TTFB targets for a portfolio on Vercel:**
- Homepage: < 100ms ✅
- Project pages (static): < 100ms ✅

If your portfolio TTFB is > 500ms, check:
1. Are you using SSR unnecessarily?
2. Is your hosting platform slow?
3. Are you making API calls in the request path?

---

*Next: Read `WEB-PERFORMANCE-METRICS.md` for other metrics →*
