# 🔧 Component Optimization — Practical Tips

---

## 1. Always Add key to List Items

```jsx
// BAD: React cannot track which items changed
{projects.map(project => (
  <ProjectCard project={project} />
))}

// GOOD: Use a unique, stable ID
{projects.map(project => (
  <ProjectCard key={project.id} project={project} />
))}

// BAD: Using index as key (breaks when list reorders)
{projects.map((project, index) => (
  <ProjectCard key={index} project={project} />
))}
```

---

## 2. Avoid Inline Object/Array Props

```jsx
// BAD: New array on every render, child always re-renders
<Chart data={[1, 2, 3]} colors={['red', 'blue']} />

// GOOD: Stable references outside component
const CHART_DATA = [1, 2, 3]
const CHART_COLORS = ['red', 'blue']

<Chart data={CHART_DATA} colors={CHART_COLORS} />
```

---

## 3. Virtualize Long Lists

If you render more than 50-100 items, use virtualization. Only renders items visible on screen.

```bash
npm install @tanstack/react-virtual
```

```jsx
import { useVirtualizer } from '@tanstack/react-virtual'

function VirtualList({ items }) {
  const parentRef = useRef(null)

  const virtualizer = useVirtualizer({
    count: items.length,
    getScrollElement: () => parentRef.current,
    estimateSize: () => 50,
  })

  return (
    <div ref={parentRef} style={{ height: '500px', overflow: 'auto' }}>
      <div style={{ height: virtualizer.getTotalSize() }}>
        {virtualizer.getVirtualItems().map(virtualRow => (
          <div
            key={virtualRow.key}
            style={{ position: 'absolute', top: virtualRow.start, height: virtualRow.size }}
          >
            {items[virtualRow.index].name}
          </div>
        ))}
      </div>
    </div>
  )
}
```

---

## 4. Split Large Components

```jsx
// BAD: One giant component — any state change re-renders everything
function ProjectPage() {
  const [filter, setFilter] = useState('all')
  return (
    <div>
      <Header />
      <FilterBar filter={filter} setFilter={setFilter} />
      <ProjectGrid filter={filter} />
      <Footer />
    </div>
  )
}

// GOOD: Header and Footer isolated — filter changes do not affect them
function ProjectPage() {
  return (
    <div>
      <Header />           // Isolated — never re-renders due to filter
      <FilterSection />    // Contains its own filter state
      <Footer />           // Isolated
    </div>
  )
}

function FilterSection() {
  const [filter, setFilter] = useState('all')
  return (
    <>
      <FilterBar filter={filter} setFilter={setFilter} />
      <ProjectGrid filter={filter} />
    </>
  )
}
```

---

## 5. Lazy Load Components Below the Fold

```jsx
import dynamic from 'next/dynamic'

// ContactForm is at the bottom of the page — load it lazily
const ContactForm = dynamic(() => import('./ContactForm'))
const ProjectsSection = dynamic(() => import('./ProjectsSection'))
```

---

## Performance Checklist for React Components

- [ ] All list items have stable, unique `key` props
- [ ] No inline object/array/function props on frequently-rendering components
- [ ] `React.memo` on expensive components that receive stable props
- [ ] Long lists (50+ items) use virtualization
- [ ] Large components are split into smaller, isolated ones
- [ ] Below-fold components use dynamic imports
