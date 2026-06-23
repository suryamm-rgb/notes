# 🔄 React Rendering — Understanding Re-renders

---

## When Does a Component Re-render?

A component re-renders when:

1. **Its own state changes** (`useState`, `useReducer`)
2. **Its props change** (parent passes new value)
3. **Its parent re-renders** (even if props did not change!)
4. **Context it subscribes to changes**

---

## Visualizing Re-renders

```jsx
// Every time App re-renders, ALL children re-render too
function App() {
  const [theme, setTheme] = useState('light')

  return (
    <div>
      <Header />           // Re-renders when theme changes
      <MainContent />      // Re-renders when theme changes
      <Footer />           // Re-renders when theme changes (even though Footer has no theme!)
    </div>
  )
}
```

This is often fine for small components. It is only a problem when components are expensive to render.

---

## Finding Unnecessary Re-renders

Add this to a component to see when it renders:
```jsx
function MyComponent({ name }) {
  console.log('MyComponent rendered:', name)
  return <div>{name}</div>
}
```

Or use React DevTools Profiler to see a flame graph of renders.

---

## State Colocation — The Best Optimization

Instead of lifting state unnecessarily, keep state close to where it is used:

```jsx
// BAD: Theme state in parent causes Header and Footer to re-render
function App() {
  const [theme, setTheme] = useState('light')
  return (
    <>
      <Header />
      <ThemeSwitcher theme={theme} setTheme={setTheme} />
      <Footer />
    </>
  )
}

// GOOD: Theme state colocated in ThemeSwitcher — only it re-renders
function ThemeSwitcher() {
  const [theme, setTheme] = useState('light')
  return <button onClick={() => setTheme(t => t === 'light' ? 'dark' : 'light')}>{theme}</button>
}

function App() {
  return (
    <>
      <Header />         // Never re-renders due to theme
      <ThemeSwitcher />
      <Footer />         // Never re-renders due to theme
    </>
  )
}
```

---

## The Rendering Pipeline

```
State/Props Change
       |
   Render Phase      (React calls your component function, gets new JSX)
       |
   Reconciliation    (React compares new JSX to previous — the "diff")
       |
   Commit Phase      (React updates the actual DOM)
       |
   Browser Paints    (User sees the update)
```

Performance optimizations primarily target the **Render Phase** — preventing unnecessary component function calls.
