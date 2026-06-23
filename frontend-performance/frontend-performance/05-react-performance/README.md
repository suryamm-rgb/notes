# ⚛️ React Performance — How React Works Under the Hood

> Understanding why React re-renders is the key to making React apps fast.

---

## How React Renders (Simplified)

1. Your component returns JSX
2. React compares it to the previous render (this is called "diffing")
3. React updates only the parts of the DOM that changed
4. This is called **reconciliation**

---

## The Problem: Unnecessary Re-renders

React re-renders a component when:
- Its **state** changes
- Its **props** change
- Its **parent** re-renders (even if nothing relevant changed!)

```jsx
function Parent() {
  const [count, setCount] = useState(0)

  return (
    <div>
      <button onClick={() => setCount(c => c + 1)}>Count: {count}</button>
      <ExpensiveChild />   // Re-renders on every count change!
    </div>
  )
}
```

`ExpensiveChild` has nothing to do with `count`, but it still re-renders every time the parent does.

---

## The Solutions

| Problem | Solution | File |
|---------|---------|------|
| Child re-renders unnecessarily | `React.memo` | `MEMOIZATION.md` |
| Expensive calculation runs every render | `useMemo` | `USE-MEMO.md` |
| New function reference on every render | `useCallback` | `USE-CALLBACK.md` |
| General component slowness | Component patterns | `COMPONENT-OPTIMIZATION.md` |

---

## The Golden Rule

> **Do not optimize prematurely.** Profile first, optimize second.

Before adding `useMemo`, `useCallback`, or `memo` — measure if there's actually a problem.

Most React performance issues come from a small number of causes:
1. Large lists without virtualization
2. Missing `key` props on list items
3. Heavy computation without memoization
4. Unnecessary state that causes cascading re-renders

---

## How to Profile React

1. Open Chrome DevTools (`F12`)
2. Go to **Profiler** tab (in React DevTools — install from Chrome Web Store)
3. Click Record
4. Interact with your app
5. Stop recording
6. Look for components with long render times (shown in flame graph)

Install React DevTools: https://chromewebstore.google.com/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi
