# 🔍 Next.js Bundle Analyzer (Official)

> The easiest way to analyze bundles in a Next.js project.

---

## Setup (5 Minutes)

### 1. Install
```bash
npm install --save-dev @next/bundle-analyzer
```

### 2. Configure next.config.js
```javascript
const withBundleAnalyzer = require('@next/bundle-analyzer')({
  enabled: process.env.ANALYZE === 'true',
})

module.exports = withBundleAnalyzer({
  // your existing Next.js config here
})
```

### 3. Add script to package.json
```json
{
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "analyze": "ANALYZE=true next build"
  }
}
```

### 4. Run
```bash
npm run analyze
```

Two tabs open automatically:
- **Client bundle** — what users download
- **Server bundle** — what runs on the server

Focus on the client bundle. That is what affects your users.

---

## What to Look For

1. The biggest rectangles — are they justified?
2. Libraries you did not intentionally add (pulled in by other packages)
3. The same code appearing in multiple chunks

---

## Quick Check Without Full Analyzer

```bash
npm run build
```

Check the terminal output:
```
Route (app)         Size     First Load JS
○ /                 5.2 kB   87.4 kB     (green = good)
○ /blog             42.1 kB  124.3 kB    (yellow = investigate)
```

If First Load JS is above 200KB on any route, run the full analyzer to find why.
