# 🖥️ SSR — Server-Side Rendering

> **In plain English:** Build the HTML on the server for every single request. Slower than static, but always fresh and can be personalized.

---

## What Is SSR?

With SSR, the server generates fresh HTML on every request:

```
User requests /dashboard
→ Server fetches user's data from database
→ Server builds HTML with that user's data
→ Sends HTML to browser
→ Browser displays it
```

---

## When to Use SSR

Use SSR when the page content depends on:
- **The logged-in user** (dashboard, profile, settings)
- **Real-time data** (live prices, live scores)
- **The request itself** (cookies, headers, search params)

Do NOT use SSR for:
- Static content (portfolio, blog — use SSG or ISR)
- Content that rarely changes (about page, contact page)

---

## How to Use SSR in Next.js App Router

```javascript
// Force dynamic (SSR) rendering
export const dynamic = 'force-dynamic'

export default async function DashboardPage() {
  const user = await getCurrentUser()  // Different for each user
  const stats = await getUserStats(user.id)

  return (
    <div>
      <h1>Welcome back, {user.name}</h1>
      <Stats data={stats} />
    </div>
  )
}
```

Or use `cache: 'no-store'` in a fetch (which also forces dynamic):
```javascript
export default async function LivePricePage() {
  const prices = await fetch('https://api.prices.com/live', {
    cache: 'no-store'  // This makes the page dynamic/SSR
  }).then(r => r.json())

  return <PriceTable prices={prices} />
}
```

---

## SSR Performance Tips

### 1. Use Streaming for Slow Data
```javascript
import { Suspense } from 'react'

export default function DashboardPage() {
  return (
    <div>
      <h1>Dashboard</h1>
      <Suspense fallback={<Skeleton />}>
        <SlowDataComponent />  {/* Renders when data is ready */}
      </Suspense>
      <FastComponent />         {/* Renders immediately */}
    </div>
  )
}
```

### 2. Parallel Data Fetching
```javascript
// BAD: Sequential (total time = request1 + request2 + request3)
const user = await getUser()
const posts = await getPosts(user.id)
const comments = await getComments(user.id)

// GOOD: Parallel (total time = slowest request)
const [user, posts, comments] = await Promise.all([
  getUser(),
  getPosts(userId),
  getComments(userId)
])
```
