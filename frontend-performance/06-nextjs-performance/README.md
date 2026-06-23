# 06 — Next.js Performance

Next.js is built for performance, but you need to know which rendering strategy to use and how to leverage caching. This folder is for Next.js 13+ App Router (the modern way).

---

## The 4 rendering strategies

| Strategy | When the HTML is built | Use for |
|----------|------------------------|---------|
| **SSG** (Static) | At build time | Blog posts, marketing pages, docs |
| **ISR** | At build, refreshed on a schedule | E-commerce catalog, news feeds |
| **SSR** | On every request | Personalized dashboards, user-specific pages |
| **CSR** | In the browser | Highly interactive widgets (graphs, editors) |

**Default to SSG/ISR.** Only use SSR when you need request-time data.

---

## 1. Server Components (RSC)

In Next.js App Router, components are **Server Components by default**. They:
- Run only on the server
- Send **zero JavaScript** to the browser
- Can directly `await` database/API calls
- Cannot use `useState`, `useEffect`, `onClick`

```tsx
// app/blog/page.tsx — Server Component (default)
export default async function BlogPage() {
  const posts = await db.posts.findMany();  // runs on server
  return <ul>{posts.map(p => <li>{p.title}</li>)}</ul>;
}
```

**Benefits:**
- Smaller JS bundle (component code never reaches the browser)
- Faster Time to First Byte (no client-side data fetching waterfall)
- Direct DB access — no API layer needed

**Rule:** Use Server Components everywhere you can.

---

## 2. Client Components

Add `"use client"` at the top of a file to make it a Client Component:

```tsx
"use client";
import { useState } from 'react';

export default function Counter() {
  const [count, setCount] = useState(0);
  return <button onClick={() => setCount(c => c + 1)}>{count}</button>;
}
```

**Use ONLY when you need:**
- Interactivity (`onClick`, `onChange`)
- State (`useState`, `useReducer`)
- Effects (`useEffect`)
- Browser APIs (`window`, `localStorage`)

**Trick:** push `"use client"` to the **leaf** components. Don't mark a whole page as client just to use one button.

```tsx
// ✅ GOOD — page is Server Component, only the button is Client
// app/page.tsx (Server)
import LikeButton from './LikeButton';
export default async function Page() {
  const post = await fetchPost();
  return <><h1>{post.title}</h1><LikeButton id={post.id} /></>;
}
```

---

## 3. Caching

Next.js caches aggressively. Understand the layers:

### `fetch()` cache (request-level)
By default, `fetch()` calls inside Server Components are cached forever:
```tsx
const res = await fetch('https://api.example.com/data');  // cached
```

Control it:
```tsx
fetch(url, { cache: 'no-store' });           // never cache
fetch(url, { next: { revalidate: 60 } });    // cache for 60 sec
fetch(url, { next: { tags: ['posts'] } });   // tag for invalidation
```

### Route segment config
```tsx
// app/blog/page.tsx
export const revalidate = 3600;  // rebuild this page every hour
```

### On-demand revalidation
```tsx
import { revalidatePath, revalidateTag } from 'next/cache';
revalidatePath('/blog');
revalidateTag('posts');
```

---

## 4. ISR — Incremental Static Regeneration

**The best of both worlds:** pages are pre-built (fast) but rebuilt periodically with fresh data.

```tsx
// app/products/[id]/page.tsx
export const revalidate = 60;  // rebuild every 60 seconds

export default async function Product({ params }) {
  const product = await fetch(`/api/product/${params.id}`).then(r => r.json());
  return <div>{product.name}</div>;
}
```

First request after 60 seconds triggers a rebuild in the background. Users always get fast cached pages.

---

## 5. Static Generation (SSG)

For pages that never change, generate them once at build time:

```tsx
// app/about/page.tsx — built once, served forever as static HTML
export default function About() {
  return <h1>About us</h1>;
}
```

For dynamic routes with a known set of values:
```tsx
// app/blog/[slug]/page.tsx
export async function generateStaticParams() {
  const posts = await fetchAllPosts();
  return posts.map(p => ({ slug: p.slug }));  // pre-build all post pages
}
```

---

## 6. Streaming with Suspense

Show parts of the page as soon as they're ready:

```tsx
import { Suspense } from 'react';

export default function Dashboard() {
  return (
    <>
      <Header />            {/* renders immediately */}
      <Suspense fallback={<Spinner />}>
        <SlowChart />       {/* streams in when ready */}
      </Suspense>
    </>
  );
}
```

Improves perceived performance — users see content faster.

---

## 7. `next/image` and `next/font`

Always use these:

### `next/image` — see `04-image-optimization`
```tsx
import Image from 'next/image';
<Image src="/hero.jpg" alt="" width={1600} height={900} priority />
```

### `next/font` — self-hosted fonts, no layout shift
```tsx
import { Inter } from 'next/font/google';
const inter = Inter({ subsets: ['latin'] });
export default function Layout({ children }) {
  return <html className={inter.className}>{children}</html>;
}
```

Zero layout shift, no external request to Google Fonts.

---

## 8. Bundle analysis (Next.js specific)

```bash
npm install -D @next/bundle-analyzer
```
```js
// next.config.js
const withBundleAnalyzer = require('@next/bundle-analyzer')({
  enabled: process.env.ANALYZE === 'true'
});
module.exports = withBundleAnalyzer({});
```
```bash
ANALYZE=true npm run build
```

---

## 9. Dynamic imports (`next/dynamic`)

For heavy client-only components (charts, editors, maps):

```tsx
import dynamic from 'next/dynamic';
const Map = dynamic(() => import('./Map'), {
  ssr: false,           // don't render on server
  loading: () => <p>Loading map...</p>,
});
```

---

## ✅ Beginner action steps

1. Audit your pages — which actually need to be dynamic? Move the rest to SSG/ISR.
2. Mark only **leaf** interactive components as `"use client"`.
3. Replace all `<img>` with `<Image>` and add `priority` to your hero image.
4. Use `next/font` for any web fonts.
5. Wrap slow data fetches in `<Suspense>` so the rest of the page renders fast.
6. Run `ANALYZE=true npm run build` and inspect your bundle.

---

## 🎓 You did it!

You've finished the guide. Here's your continuing-education path:

- 📖 **Web.dev** — <https://web.dev/learn/> (free Google courses)
- 📖 **Next.js docs** — <https://nextjs.org/docs>
- 🎥 **Vercel YouTube** — talks on real-world performance
- 🛠 **WebPageTest** — <https://www.webpagetest.org/> for deep waterfall analysis

**Remember the loop:**
1. **Measure** (Lighthouse / PageSpeed Insights)
2. **Identify** the biggest opportunity
3. **Fix** one thing
4. **Re-measure**
5. Repeat

Good luck with your portfolio! 🚀
