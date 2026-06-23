# 💻 Client Components — When to Use "use client"

> **In plain English:** Add "use client" only when a component needs interactivity or browser APIs.

---

## What "use client" Does

It marks a component as a Client Component — meaning:
- Its JavaScript is sent to the browser
- It can use React hooks (useState, useEffect, useRef, etc.)
- It can handle user events (onClick, onChange, etc.)
- It can access browser APIs (window, localStorage, etc.)

---

## When You NEED "use client"

```jsx
// Needs useState → use client
'use client'
import { useState } from 'react'
export function Counter() {
  const [count, setCount] = useState(0)
  return <button onClick={() => setCount(c => c + 1)}>{count}</button>
}

// Needs useEffect → use client
'use client'
import { useEffect } from 'react'
export function Analytics() {
  useEffect(() => {
    trackPageView()
  }, [])
  return null
}

// Needs browser API → use client
'use client'
export function ThemeSwitcher() {
  const theme = window.localStorage.getItem('theme')
  // ...
}
```

---

## When You Do NOT Need "use client"

```jsx
// Just displays data — no hooks needed, keep as Server Component
export function ProjectCard({ title, description, imageUrl }) {
  return (
    <div>
      <Image src={imageUrl} alt={title} width={400} height={300} />
      <h3>{title}</h3>
      <p>{description}</p>
    </div>
  )
}

// Fetches and displays data — Server Component
export async function ProjectList() {
  const projects = await getProjects()
  return projects.map(p => <ProjectCard key={p.id} {...p} />)
}
```

---

## The "Boundary" Rule

When you add "use client" to a component, ALL its children automatically become Client Components too (unless they are imported and passed as props).

```jsx
// ClientWrapper.jsx — "use client" makes everything inside client-side
'use client'
export function ClientWrapper({ children }) {
  const [open, setOpen] = useState(false)
  return (
    <div>
      <button onClick={() => setOpen(!open)}>Toggle</button>
      {open && children}
    </div>
  )
}

// page.jsx — ServerComponent can pass Server Component children into Client Component
export default function Page() {
  return (
    <ClientWrapper>
      <HeavyServerComponent />  {/* This stays a Server Component! */}
    </ClientWrapper>
  )
}
```

---

## Rule of Thumb

Push "use client" as far DOWN the component tree as possible.

```
Page (Server)
  └── Layout (Server)
        ├── Header (Server)        ← Static nav links, no interactivity
        │     └── MobileMenu (Client)  ← Only this needs useState for open/closed
        ├── Main (Server)
        └── Footer (Server)        ← Static links, no interactivity
```
