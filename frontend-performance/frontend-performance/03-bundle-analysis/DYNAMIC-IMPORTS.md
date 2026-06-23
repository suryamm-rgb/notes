# ⚡ Dynamic Imports — Load Code On Demand

> **In plain English:** Do not load code at startup. Wait until the user actually needs it.

---

## Static vs Dynamic Import

```javascript
// Static: always loaded on page start, even if user never needs it
import { parseCSV } from 'csv-parser'

// Dynamic: loaded only the first time this function runs
async function handleUpload(file) {
  const { parseCSV } = await import('csv-parser')
  return parseCSV(file)
}
```

---

## Next.js dynamic() — The Recommended Way

```javascript
import dynamic from 'next/dynamic'

// Basic
const Chart = dynamic(() => import('./Chart'))

// With loading skeleton
const Chart = dynamic(() => import('./Chart'), {
  loading: () => <Skeleton height={400} />,
})

// Disable server-side rendering (for browser-only libraries)
const Map = dynamic(() => import('./Map'), {
  ssr: false,
})
```

---

## Common Use Cases

### Heavy chart libraries
```javascript
const RechartsChart = dynamic(() => import('./RechartsChart'), {
  ssr: false,
  loading: () => <div style={{ height: 300, background: '#f0f0f0' }} />
})
```

### Code editors (Monaco, CodeMirror)
```javascript
const CodeEditor = dynamic(() => import('@monaco-editor/react'), {
  ssr: false,
  loading: () => <p>Loading editor...</p>
})
```

### Modals — only load when opened
```javascript
const [showModal, setShowModal] = useState(false)
const Modal = dynamic(() => import('./Modal'))

return (
  <>
    <button onClick={() => setShowModal(true)}>Open Modal</button>
    {showModal && <Modal onClose={() => setShowModal(false)} />}
  </>
)
// Modal JS is not downloaded until showModal becomes true
```

### Contact form below the fold
```javascript
// Form code only loads when user scrolls down to it
const ContactForm = dynamic(() => import('./ContactForm'))
```

---

## Preloading — Load in Background Before Needed

```javascript
const Modal = dynamic(() => import('./Modal'))

function App() {
  // Start downloading modal code when user hovers (before they click)
  const handleMouseEnter = () => {
    import('./Modal')
  }

  return (
    <button onMouseEnter={handleMouseEnter} onClick={openModal}>
      Open Modal
    </button>
  )
}
```

This gives you lazy loading AND fast perceived performance.
