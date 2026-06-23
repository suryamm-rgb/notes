# 💾 Next.js Caching — How It Works

> Next.js has multiple layers of caching. Understanding them helps you build fast apps and avoid stale data bugs.

---

## The 4 Caching Mechanisms

| Cache | What It Caches | Default | Where |
|-------|---------------|---------|-------|
| Request Memoization | Duplicate `fetch` calls | Automatic | Per request |
| Data Cache | `fetch` responses | Indefinite | Server |
| Full Route Cache | Rendered HTML | Indefinite | Server |
| Router Cache | Client navigation | 30 seconds | Browser |

---

## Data Cache — Controlling How Long Data is Cached

```javascript
// Cache forever (default for static pages)
const data = await fetch('https://api.example.com/projects')

// Cache for 1 hour, then revalidate
const data = await fetch('https://api.example.com/projects', {
  next: { revalidate: 3600 }
})

// Never cache (always fresh)
const data = await fetch('https://api.example.com/user', {
  cache: 'no-store'
})
```

---

## When to Use Which Cache Setting

| Content Type | Cache Setting | Why |
|-------------|--------------|-----|
| Portfolio projects | Default (forever) | Never changes |
| Blog posts | `revalidate: 3600` | Updates occasionally |
| GitHub stars | `revalidate: 86400` | Updates daily is fine |
| User profile | `cache: 'no-store'` | Must be fresh |
| Stock prices | `cache: 'no-store'` | Must be real-time |

---

## Revalidating Cache Manually (ISR on-demand)

```javascript
// app/api/revalidate/route.js
import { revalidatePath, revalidateTag } from 'next/cache'

export async function POST(request) {
  const { path, secret } = await request.json()

  if (secret !== process.env.REVALIDATE_SECRET) {
    return Response.json({ message: 'Invalid secret' }, { status: 401 })
  }

  revalidatePath(path)  // Revalidate a specific path
  // OR
  revalidateTag('projects')  // Revalidate all fetches tagged with 'projects'

  return Response.json({ revalidated: true })
}
```

Call this from your CMS webhook when content changes.

---

## Tagging Fetches for Targeted Revalidation

```javascript
// Tag this fetch
const projects = await fetch('https://api.example.com/projects', {
  next: { tags: ['projects'] }
})

// Later, invalidate only this tag
revalidateTag('projects')  // Only refetches tagged data
```

---

## Common Caching Mistakes

```javascript
// MISTAKE: Caching user-specific data
const user = await fetch('/api/user', {
  // No cache: 'no-store' — this caches user data and serves it to everyone!
})
// Fix: always use cache: 'no-store' for user-specific endpoints

// MISTAKE: Not caching static data
const projects = await fetch('/api/projects', {
  cache: 'no-store'  // Fetches from API on every request — unnecessary
})
// Fix: remove cache: 'no-store' for stable data, or add revalidate
```
