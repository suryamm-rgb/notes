# 🔍 Webpack Bundle Analyzer

> Visualize what is inside your JavaScript bundle as an interactive treemap.

---

## Install

```bash
npm install --save-dev webpack-bundle-analyzer
```

---

## Setup in Next.js

```javascript
// next.config.js
const { BundleAnalyzerPlugin } = require('webpack-bundle-analyzer')

module.exports = {
  webpack: (config, { isServer }) => {
    if (process.env.ANALYZE === 'true') {
      config.plugins.push(
        new BundleAnalyzerPlugin({
          analyzerMode: 'static',
          reportFilename: isServer ? '../analyze/server.html' : './analyze/client.html',
          openAnalyzer: true,
        })
      )
    }
    return config
  },
}
```

```bash
ANALYZE=true npm run build
```

---

## Reading the Treemap

- Bigger box = more bytes
- Hover to see exact sizes
- Click to zoom in

Look for:
- Unexpectedly large boxes
- Duplicated packages (same library twice)
- Sections of libraries you barely use

---

## Common Findings and Fixes

| What You See | Fix |
|-------------|-----|
| `moment.js` (67KB) | Replace with `date-fns` |
| All of `lodash` (70KB) | Use `lodash-es` with named imports |
| Duplicate React | Run `npm ls react` and fix version conflicts |
| Huge icon library | Import specific icons only |
