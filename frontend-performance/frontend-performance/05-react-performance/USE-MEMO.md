# 🔢 useMemo — Cache Expensive Calculations

> **Use when:** You have an expensive calculation inside a component that should not re-run on every render.

---

## Basic Syntax

```jsx
const memoizedValue = useMemo(() => {
  return expensiveCalculation(dependency1, dependency2)
}, [dependency1, dependency2])
//   ^^ Array of dependencies — recalculates only when these change
```

---

## When to Use useMemo

```jsx
// BAD: Filters 10,000 items on every render
function ProjectList({ projects, filter }) {
  const filtered = projects.filter(p => p.category === filter)
  // ^ Runs on EVERY render, even if projects and filter did not change

  return filtered.map(p => <ProjectCard key={p.id} {...p} />)
}

// GOOD: Only re-filters when projects or filter actually changes
function ProjectList({ projects, filter }) {
  const filtered = useMemo(
    () => projects.filter(p => p.category === filter),
    [projects, filter]
  )

  return filtered.map(p => <ProjectCard key={p.id} {...p} />)
}
```

---

## Portfolio Use Cases

### Sorting projects
```jsx
function Projects({ projects, sortBy }) {
  const sorted = useMemo(() => {
    return [...projects].sort((a, b) => {
      if (sortBy === 'date') return new Date(b.date) - new Date(a.date)
      if (sortBy === 'name') return a.name.localeCompare(b.name)
      return 0
    })
  }, [projects, sortBy])

  return sorted.map(p => <ProjectCard key={p.id} project={p} />)
}
```

### Filtering by tag
```jsx
function Projects({ projects, activeTag }) {
  const filtered = useMemo(() => {
    if (!activeTag) return projects
    return projects.filter(p => p.tags.includes(activeTag))
  }, [projects, activeTag])

  return filtered.map(p => <ProjectCard key={p.id} project={p} />)
}
```

---

## When NOT to Use useMemo

```jsx
// OVERKILL: Simple calculation, no need for useMemo
const doubled = useMemo(() => count * 2, [count])
// Just write:
const doubled = count * 2

// OVERKILL: Component renders rarely anyway
function StaticHeader({ title }) {
  const titleCase = useMemo(() => toTitleCase(title), [title])
  // toTitleCase is fast, header renders once — no benefit
}
```

Rule of thumb: If the calculation takes less than 1ms, skip `useMemo`.

---

## useMemo vs useCallback

| Hook | Caches | Use For |
|------|--------|---------|
| `useMemo` | A **value** (result of calculation) | Expensive computations |
| `useCallback` | A **function** | Event handlers passed to child components |
