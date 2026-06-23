# 🧠 Memoization — Remember Results to Skip Work

> **In plain English:** Memoization means "remember the result of a calculation and reuse it instead of recalculating."

---

## What Is Memoization?

```javascript
// Without memoization: recalculates every time
function getTotal(items) {
  return items.reduce((sum, item) => sum + item.price, 0)
}

// With memoization: returns cached result if items did not change
const getTotal = memoize((items) => {
  return items.reduce((sum, item) => sum + item.price, 0)
})
```

In React, memoization prevents components from re-rendering and calculations from re-running when nothing relevant changed.

---

## React.memo — Memoize Entire Components

```jsx
import { memo } from 'react'

// Without memo: re-renders whenever parent re-renders
function ProjectCard({ title, description }) {
  return (
    <div>
      <h3>{title}</h3>
      <p>{description}</p>
    </div>
  )
}

// With memo: only re-renders if title or description changes
const ProjectCard = memo(function ProjectCard({ title, description }) {
  return (
    <div>
      <h3>{title}</h3>
      <p>{description}</p>
    </div>
  )
})
```

---

## When to Use React.memo

Use it when:
- Component renders frequently (inside lists, often-updating parents)
- Component is expensive to render (complex UI, heavy calculations)
- Props rarely change

Do NOT use it when:
- Component is simple and cheap to render
- Props change on almost every render anyway (memoization overhead exceeds savings)

---

## The Trap: Object and Function Props

`React.memo` uses shallow comparison. Objects and functions create a new reference on every render, breaking memo.

```jsx
// BAD: New object on every render defeats memo
function Parent() {
  return (
    <MemoizedChild
      style={{ color: 'red' }}    // New object every render!
      onClick={() => handleClick()} // New function every render!
    />
  )
}

// GOOD: Stable references
const CARD_STYLE = { color: 'red' }  // Created once, outside component

function Parent() {
  const handleClick = useCallback(() => { /* ... */ }, [])  // Stable function

  return <MemoizedChild style={CARD_STYLE} onClick={handleClick} />
}
```

See `USE-CALLBACK.md` for how to create stable function references.
