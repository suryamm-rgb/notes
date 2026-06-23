# 🖥️ React Server Components (RSC)

> **In plain English:** Server Components run on the server and send HTML to the browser — no JavaScript is sent for them. This makes your bundle smaller.

---

## Server vs Client Components

| | Server Component | Client Component |
|--|----------------|-----------------|
| Runs on | Server | Browser |
| JavaScript sent to browser | None | Yes |
| Can use hooks (useState, useEffect) | No | Yes |
| Can directly access database | Yes | No |
| Can handle user interactions | No | Yes |

---

## Default in Next.js App Router

In the `app/` directory, all components are **Server Components by default**.
You opt into Client Components by adding `"use client"` at the top.

```jsx
// app/page.js — Server Component by default (no "use client")
export default async function HomePage() {
  // This runs on the server — user never downloads this code!
  const projects = await fetch('https://api.example.com/projects').then(r => r.json())

  return (
    <main>
      <h1>My Portfolio</h1>
      {projects.map(p => <ProjectCard key={p.id} project={p} />)}
    </main>
  )
}
```

---

## Benefits for Portfolio Sites

```jsx
// BEFORE (Pages Router): Fetching in getStaticProps, more boilerplate
export async function getStaticProps() {
  const projects = await fetchProjects()
  return { props: { projects } }
}
export default function Projects({ projects }) { ... }

// AFTER (App Router with Server Components): Simpler, faster
export default async function Projects() {
  const projects = await fetchProjects()  // Direct, no boilerplate
  return projects.map(p => <ProjectCard key={p.id} project={p} />)
}
```

---

## Server Component + Client Component Pattern

```jsx
// app/projects/page.js — Server Component (fetches data)
import ProjectFilter from './ProjectFilter'  // Client Component

export default async function ProjectsPage() {
  const projects = await fetchProjects()  // Server only

  return (
    <div>
      <h1>Projects</h1>
      <ProjectFilter projects={projects} />  {/* Interactive filtering is client-side */}
    </div>
  )
}

// app/projects/ProjectFilter.js — Client Component (handles interactions)
'use client'
import { useState } from 'react'

export default function ProjectFilter({ projects }) {
  const [filter, setFilter] = useState('all')
  const filtered = filter === 'all' ? projects : projects.filter(p => p.type === filter)

  return (
    <>
      <select value={filter} onChange={e => setFilter(e.target.value)}>
        <option value="all">All</option>
        <option value="web">Web</option>
        <option value="mobile">Mobile</option>
      </select>
      {filtered.map(p => <ProjectCard key={p.id} project={p} />)}
    </>
  )
}
```

The data fetching happens on the server (fast, no API key exposed). The filtering happens on the client (interactive).
