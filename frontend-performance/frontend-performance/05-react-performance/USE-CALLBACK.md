# 📞 useCallback — Stable Function References

> **Use when:** You pass a function as a prop to a memoized child component, and you want the child to not re-render because the function changes.

---

## The Problem Without useCallback

```jsx
const MemoizedButton = memo(function Button({ onClick, label }) {
  console.log('Button rendered')
  return <button onClick={onClick}>{label}</button>
})

function Parent() {
  const [count, setCount] = useState(0)

  // BAD: New function created on every render
  const handleClick = () => {
    console.log('clicked')
  }

  return (
    <>
      <button onClick={() => setCount(c => c + 1)}>Count: {count}</button>
      <MemoizedButton onClick={handleClick} label="Click me" />
      {/* MemoizedButton STILL re-renders because handleClick is a new function each time */}
    </>
  )
}
```

## The Solution With useCallback

```jsx
function Parent() {
  const [count, setCount] = useState(0)

  // GOOD: Same function reference across renders (unless dependencies change)
  const handleClick = useCallback(() => {
    console.log('clicked')
  }, [])  // Empty array = never recreated

  return (
    <>
      <button onClick={() => setCount(c => c + 1)}>Count: {count}</button>
      <MemoizedButton onClick={handleClick} label="Click me" />
      {/* MemoizedButton does NOT re-render now */}
    </>
  )
}
```

---

## With Dependencies

```jsx
function TodoList({ todos, userId }) {
  const handleDelete = useCallback((todoId) => {
    deleteTodo(userId, todoId)
  }, [userId])  // Re-created only when userId changes
  // ^ Not on every render

  return todos.map(todo => (
    <TodoItem key={todo.id} todo={todo} onDelete={handleDelete} />
  ))
}
```

---

## When to Use useCallback

Use it when:
- Passing a function to a `memo`-wrapped component
- Function is a dependency in another `useEffect` or `useMemo`

Skip it when:
- The function is used only within the component itself
- The child component is not memoized (useCallback has no effect)
- The function dependencies change on every render anyway

---

## Summary: When to Use Which Hook

```jsx
// Cache a VALUE  → useMemo
const expensiveResult = useMemo(() => heavyCalc(data), [data])

// Cache a FUNCTION → useCallback
const stableHandler = useCallback(() => doSomething(id), [id])

// Prevent COMPONENT re-render → React.memo
const OptimizedChild = memo(function Child({ value }) { ... })
```
