# React Hooks Notes

### Hook Learning Order

1.  useState
2.  useEffect
3.  useRef
4.  useContext
5.  useReducer
6.  useMemo
7.  useCallback
8.  useLayoutEffect
9.  useTransition
10. useDeferredValue
11. useSyncExternalStore
12. useImperativeHandle
13. useInsertionEffect
14. useActionState
15. useFormState

## useState

Purpose: Manage component state.

Syntax:

```jsx
const [state, setState] = useState(initialValue);
```

Use cases: - Forms - Counters - UI state

## useEffect

Purpose: Handle side effects after rendering.

Syntax:

```jsx
useEffect(() => {}, [dependency]);
```

Use cases: - API calls - Timers - Event listeners

## useRef

Purpose: Store values between renders without causing re-render.

Syntax:

```jsx
const ref = useRef(initialValue);
```

Use cases: - DOM access - Previous values - Timers

## useContext

Purpose: Share data between components without prop drilling.

Use cases: - Authentication - Theme - Global settings

## useReducer

Purpose: Manage complex state logic.

Syntax:

```jsx
const [state, dispatch] = useReducer(reducer, initialState);
```

Use cases: - Complex forms - Large state management

## useMemo

Purpose: Cache expensive calculations.

Use cases: - Performance optimization

## useCallback

Purpose: Memoize functions.

Use cases: - Prevent unnecessary child renders

## useImperativeHandle

Purpose: Customize values exposed through refs.

Use cases: - Custom component APIs

## useLayoutEffect

Purpose: Run effects before browser paint.

Use cases: - DOM measurements - Layout changes

## useTransition

Purpose: Mark updates as non-urgent.

Use cases: - Keep UI responsive - Heavy rendering

## useDeferredValue

Purpose: Delay updating non-critical values.

Use cases: - Search optimization - Large lists

## useSyncExternalStore

Purpose: Subscribe to external stores.

Use cases: - Redux - Zustand - Custom state stores

## useInsertionEffect

Purpose: Run before DOM mutations.

Use cases: - CSS-in-JS libraries

## useActionState

Purpose: Manage action-based state updates.

Use cases: - Server actions - Form submissions

## useFormState

Purpose: Manage form state with actions.

Use cases: - Form validation - Server-side forms
