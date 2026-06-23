# 👆 INP — Interaction to Next Paint

> **In plain English:** When a user clicks, taps, or types — how fast does something visibly happen?

---

## What Is INP?

INP (Interaction to Next Paint) measures the **responsiveness** of your page to user interactions.

Every time a user:
- Clicks a button
- Taps a menu item
- Types in a search box
- Selects a dropdown option

...there is a delay between that action and the next visible update on screen. INP measures the **worst** of these delays across the whole session.

> **Note:** INP replaced FID (First Input Delay) as an official Core Web Vital in March 2024.

---

## 🎯 INP Targets

| Score | Time |
|-------|------|
| 🟢 Good | ≤ 200ms |
| 🟡 Needs Improvement | 200 – 500ms |
| 🔴 Poor | > 500ms |

**Why 200ms?** Human perception research shows interactions that respond within 200ms feel "instant." Beyond 500ms, it feels broken.

---

## 🔍 How to Measure INP

### Chrome DevTools:
1. Open DevTools (`F12`)
2. Go to **Performance** tab
3. Check **Web Vitals** checkbox
4. Record → interact with your page → stop
5. Look for INP marker in the timeline

### In the Console (Live):
```javascript
// Paste this in Chrome DevTools Console to see INP live
new PerformanceObserver((list) => {
  for (const entry of list.getEntries()) {
    console.log('INP candidate:', entry.duration, 'ms', entry);
  }
}).observe({ type: 'event', buffered: true, durationThreshold: 16 });
```

---

## ❌ Common Causes of Bad INP

### Problem 1: Heavy JavaScript running on click
```javascript
// ❌ BAD: Doing expensive work synchronously on click
function handleClick() {
  const result = expensiveCalculation(largeDataset) // Blocks the main thread
  setResult(result)
}

// ✅ GOOD: Move heavy work off the main thread
function handleClick() {
  // Show loading state immediately (user sees feedback)
  setLoading(true)
  
  // Do heavy work asynchronously
  setTimeout(() => {
    const result = expensiveCalculation(largeDataset)
    setResult(result)
    setLoading(false)
  }, 0)
}
```

### Problem 2: Too many event listeners on scroll
```javascript
// ❌ BAD: Heavy work on every scroll event
window.addEventListener('scroll', () => {
  doExpensiveWork() // Fires hundreds of times per second
})

// ✅ GOOD: Throttle scroll events
import { throttle } from 'lodash'

window.addEventListener('scroll', throttle(() => {
  doExpensiveWork() // Fires at most once every 100ms
}, 100))
```

### Problem 3: Large React component re-rendering on every interaction
```javascript
// ❌ BAD: Entire list re-renders when anything changes
function App() {
  const [count, setCount] = useState(0)
  return (
    <div>
      <button onClick={() => setCount(c => c + 1)}>Click</button>
      <HugeExpensiveList /> {/* Re-renders on every click! */}
    </div>
  )
}

// ✅ GOOD: Memoize the expensive component
import { memo } from 'react'

const HugeExpensiveList = memo(function HugeExpensiveList() {
  return <div>{/* huge list */}</div>
})
// Now it only re-renders if its own props change
```

---

## ✅ INP Checklist for Portfolio Sites

- [ ] Click handlers do minimal work synchronously
- [ ] No heavy loops or calculations triggered by user input
- [ ] `React.memo` used on large list components
- [ ] Animations use CSS (not JS) wherever possible
- [ ] No `useEffect` with heavy work triggered by frequent state changes

---

## 💡 The Golden Rule of INP

> **Show feedback within 50ms, then do the work.**

Users don't expect instant results — they expect **acknowledgment** that something happened.

```javascript
// ✅ BEST PRACTICE: Show feedback immediately, do work after
async function handleSubmit() {
  setButtonState('loading')         // User sees spinner immediately (< 50ms)
  await submitForm(formData)         // Actual work happens after
  setButtonState('success')          // Show result
}
```

---

*Next: Read `CLS.md` →*
