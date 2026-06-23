# 🔄 ISR — Incremental Static Regeneration

> **In plain English:** Build pages statically at deploy time, but automatically rebuild them in the background when the data gets old.

---

## What Is ISR?

ISR combines the speed of static pages with the freshness of server-rendered pages.

```
First request:      Serve pre-built HTML instantly (fast)
After X seconds:    Rebuild the page in the background
Next request:       Serve the new version
```

Users always get a fast response. The page stays fresh.

---

## How to Use ISR in Next.js App Router

```javascript
// app/projects/page.js
export const revalidate = 3600  // Rebuild this page every hour

export default async function ProjectsPage() {
  const projects = await fetch('https://api.example.com/projects').then(r => r.json())
  return projects.map(p => <ProjectCard key={p.id} project={p} />)
}
```

That single export `revalidate` is all you need.

---

## ISR with Dynamic Routes

```javascript
// app/projects/[slug]/page.js

// Generate static pages for known slugs at build time
export async function generateStaticParams() {
  const projects = await getProjects()
  return projects.map(p => ({ slug: p.slug }))
}

export const revalidate = 86400  // Rebuild each project page every 24 hours

export default async function ProjectPage({ params }) {
  const project = await getProject(params.slug)
  return <ProjectDetail project={project} />
}
```

---

## Revalidation Times Guide

| Content | Revalidate Every |
|---------|-----------------|
| Portfolio projects | 86400 (24h) or never |
| Blog posts | 3600 (1h) |
| GitHub stats | 3600 (1h) |
| News/events | 300 (5 min) |
| Stock prices | 0 (use SSR instead) |

---

## On-Demand ISR (Trigger Rebuild from CMS)

Instead of time-based revalidation, trigger a rebuild when content changes:

```javascript
// app/api/revalidate/route.js
import { revalidatePath } from 'next/cache'

export async function POST(request) {
  revalidatePath('/projects')  // Rebuild this specific route
  return Response.json({ revalidated: true })
}
```

Set this URL as a webhook in your CMS (Contentful, Sanity, Notion, etc.).
