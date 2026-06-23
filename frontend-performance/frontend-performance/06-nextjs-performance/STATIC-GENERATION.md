# ⚡ Static Generation (SSG) — The Fastest Option

> **In plain English:** Build all your HTML pages at deploy time. When users visit, they get pre-built HTML instantly — no server processing needed.

---

## What Is Static Generation?

```
At build time:  Next.js fetches data + builds all HTML pages
At runtime:     CDN serves pre-built HTML instantly to users
```

This is the fastest possible way to serve a web page. The server does zero work per request.

---

## Default Behavior in Next.js App Router

By default, Next.js statically generates pages when possible. If you do not fetch dynamic data or use dynamic functions, your page is automatically static.

```javascript
// app/about/page.js — Automatically static (no data fetching)
export default function AboutPage() {
  return (
    <main>
      <h1>About Me</h1>
      <p>I am a frontend developer from Bengaluru.</p>
    </main>
  )
}
```

Built once. Served from CDN. Blazing fast.

---

## Static Generation with Data

```javascript
// app/projects/page.js
// No export dynamic needed — data is fetched at build time by default

export default async function ProjectsPage() {
  // This fetch runs at BUILD TIME, not at request time
  const projects = await fetch('https://api.github.com/users/yourusername/repos').then(r => r.json())

  return (
    <div>
      <h1>My Projects</h1>
      {projects.map(p => (
        <ProjectCard key={p.id} name={p.name} description={p.description} />
      ))}
    </div>
  )
}
```

---

## Static Generation for Dynamic Routes

```javascript
// app/projects/[slug]/page.js

// Tell Next.js which slugs to pre-build
export async function generateStaticParams() {
  const projects = await getProjects()

  return projects.map(project => ({
    slug: project.slug,
  }))
}

// This page is built for EACH slug returned above
export default async function ProjectPage({ params }) {
  const project = await getProject(params.slug)
  return <ProjectDetail project={project} />
}
```

If you have 10 projects, Next.js builds 10 HTML files at deploy time.

---

## Portfolio Architecture: Fully Static

```
app/
  page.js                  → Static (hero, intro)
  about/page.js             → Static (about me content)
  projects/page.js          → Static (project list, fetched from JSON/API at build)
  projects/[slug]/page.js   → Static x N (one page per project)
  contact/page.js           → Static (contact form, form submission via API route)
```

This gives you a 100% static portfolio with:
- Perfect Lighthouse scores
- TTFB < 100ms (served from CDN edge)
- Zero server costs (Vercel free tier is plenty)

---

## Force Static (Override Dynamic Detection)

```javascript
export const dynamic = 'force-static'  // Never SSR this page
```

---

## Static vs ISR vs SSR — Decision Summary

```
Page never changes            → Static Generation (default)
Page changes occasionally     → ISR (revalidate: 3600)
Page is different per user    → SSR (dynamic = 'force-dynamic')
Page needs real-time data     → SSR with no-store
```

For a portfolio: **Static Generation for everything.**
