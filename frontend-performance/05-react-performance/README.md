# 05 — React Performance

React is fast by default, but as your app grows, **unnecessary re-renders** can slow it down. This folder covers the patterns that keep React apps snappy.

---

## How React renders (in 30 seconds)

1. A component renders → React builds a "virtual DOM" (a JS object describing the UI)
2. React compares it to the previous virtual DOM (**diffing**)
3. Only the differences are applied to the real DOM

**The slow part is step 1.** If a component renders 1000 times when it only needed to render twice, that's wasted CPU.

A component re-renders when:
- Its **state** changes (`useState`)
- Its **props** change
- Its **parent** re-renders (this is the surprise one!)
- A **context** it consumes changes

---

## How to spot performance issues

### Tool 1: React DevTools Profiler
1. Install **React Developer Tools** Chrome extension
2. Open DevTools → **Profiler** tab
3. Click record → interact with your app → stop
4. See a flamegraph of which components rendered and how long they took

Look for:
- Components rendering when they don't need to
- Components taking >16ms to render (causes jank)

### Tool 2: "Why did you render?"
```bash
npm install @welldone-software/why-did-you-render
```
Logs to the console every time a component re-renders unnecessarily.

### Tool 3: Chrome Performance tab
Records the entire main thread. Useful for finding long tasks blocking INP.

---

## 1. Memoization — `React.memo`

Wraps a component so it **only re-renders when its props actually change**:

```tsx
const ExpensiveCard = React.memo(function ExpensiveCard({ user }) {
  return <div>{user.name}</div>;
});
```

**Use when:**
- Component renders often with the same props
- Rendering is expensive (big list, heavy computation, chart)

**Don't use when:**
- Component is cheap to render (memo has overhead too)
- Props change every render anyway

---

## 2. `useMemo` — cache expensive calculations

```tsx
const sortedUsers = useMemo(
  () => users.sort((a, b) => a.name.localeCompare(b.name)),
  [users]   // only re-sort when `users` changes
);
```

**Use when:**
- Calculation is genuinely expensive (sorting/filtering thousands of items)
- The result is passed to a memoized child

**Don't use for:** `useMemo(() => x + 1, [x])` — the memo cost > the calculation cost.

---

## 3. `useCallback` — stabilize function references

Every render creates a new function. If you pass that function to a memoized child, the child re-renders anyway because `oldFn !== newFn`.

```tsx
const handleClick = useCallback(() => {
  setCount(c => c + 1);
}, []);   // function reference stays stable

<MemoizedButton onClick={handleClick} />
```

**Use when:** passing callbacks to memoized children, or to `useEffect` dependencies.

---

## 4. Component splitting

A common mistake — putting state at the top of a huge component:

```tsx
// ❌ BAD — typing in the input re-renders the whole page
function Page() {
  const [text, setText] = useState('');
  return (
    <>
      <HeavyChart />        {/* re-renders on every keystroke! */}
      <ExpensiveSidebar />  {/* same */}
      <input value={text} onChange={e => setText(e.target.value)} />
    </>
  );
}

// ✅ GOOD — isolate the state
function Page() {
  return (
    <>
      <HeavyChart />
      <ExpensiveSidebar />
      <SearchInput />   {/* state lives in here */}
    </>
  );
}
```

**Rule:** push state down to the smallest component that needs it.

---

## 5. List virtualization

Rendering 10,000 rows? Don't. Render only the visible ones using a virtualization library:
- **TanStack Virtual** (recommended)
- **react-window**

Renders 20 rows at a time as the user scrolls. Goes from "frozen browser" to "buttery smooth."

---

## 6. Avoid inline objects/arrays in props

```tsx
// ❌ creates a new object every render → memoized child re-renders
<Child style={{ color: 'red' }} options={[1, 2, 3]} />

// ✅ define outside or memoize
const style = { color: 'red' };
const options = [1, 2, 3];
<Child style={style} options={options} />
```

---

## 7. Defer heavy work

Use `useDeferredValue` and `useTransition` (React 18+) to keep the UI responsive:

```tsx
const [filter, setFilter] = useState('');
const deferredFilter = useDeferredValue(filter);
// expensive filtering uses deferredFilter — input stays snappy
```

---

## 8. Don't over-optimize

The biggest mistake juniors make: wrapping **everything** in `useMemo`/`useCallback`/`memo`.

These tools have overhead. Adding them where they're not needed makes your code **slower and harder to read**.

**Workflow:**
1. Build it simply first
2. Profile with React DevTools
3. Memoize ONLY the components/values shown to be expensive

---

## ✅ Beginner action steps

1. Install React DevTools and open the Profiler tab.
2. Record a typical interaction in your app (click around, type, scroll).
3. Find the component with the longest render time.
4. Apply ONE of: `React.memo`, `useMemo`, or split it into smaller components.
5. Re-profile. Confirm the improvement.

> 💡 If you don't see performance issues yet, **don't optimize**. Premature optimization wastes time. Profile first, then fix what's actually slow.

**Next:** `06-nextjs-performance/README.md` — Next.js–specific tricks.
