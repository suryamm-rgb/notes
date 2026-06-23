# 📏 JavaScript Bundle Size

---

## What Is a Good Bundle Size?

| Bundle Type | Target |
|-------------|--------|
| Initial JS (first load) | < 200KB gzipped |
| Per-route JS | < 50KB gzipped |
| Total JS | < 500KB gzipped |

"Gzipped" means after compression. Web servers compress files before sending them.

---

## How to Check Bundle Size in Next.js

```bash
npm run build
```

Output example:
```
Route (app)                Size    First Load JS
○ /                        5.2 kB  87.4 kB      (Good)
○ /about                   2.1 kB  84.3 kB      (Good)
○ /projects                8.4 kB  90.6 kB      (Good)
```

- Green  = Good
- Yellow = Acceptable
- Red    = Needs attention

---

## Common Bundle Bloat Culprits

### Importing entire libraries
```javascript
// BAD: Loads all 70KB of lodash
import _ from 'lodash'
_.debounce(fn, 300)

// GOOD: Loads only 2KB debounce function
import debounce from 'lodash/debounce'
debounce(fn, 300)
```

### Moment.js (notorious bundle killer)
```javascript
// BAD: moment.js = 67KB gzipped!
import moment from 'moment'

// GOOD: date-fns loads only what you use
import { format, parseISO } from 'date-fns'
```

### Large icon libraries
```javascript
// BAD: Imports ALL icons from the entire library
import * as Icons from 'react-icons/fa'

// GOOD: Import only what you need
import { FaGithub, FaLinkedin } from 'react-icons/fa'
```
