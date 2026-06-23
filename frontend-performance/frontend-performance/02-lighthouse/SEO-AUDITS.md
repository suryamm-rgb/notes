# 🔍 SEO Audits — Get Found on Google

> SEO = Search Engine Optimization. It helps Google understand and rank your portfolio.

---

## 📄 "Document does not have a meta description"

The meta description appears under your page title in Google search results.

```jsx
// app/layout.js or app/page.js (Next.js App Router)
export const metadata = {
  title: 'John Doe — Frontend Developer',
  description: 'Frontend developer specializing in React and Next.js. View my projects and get in touch.',
}
```

```html
<!-- Plain HTML -->
<head>
  <meta name="description" content="Frontend developer specializing in React and Next.js. View my projects." />
</head>
```

**Tips:**
- 150–160 characters max
- Include your name and specialty
- Make it compelling (it's your ad copy in Google)

---

## 📰 "Document does not have a title element"

```jsx
// Next.js App Router
export const metadata = {
  title: 'John Doe — Frontend Developer',
}

// Each page can have its own title
// app/projects/page.js
export const metadata = {
  title: 'Projects — John Doe',
}
```

---

## 🔗 "Links are not crawlable"

Google must be able to follow your links.

```html
<!-- ❌ BAD: JavaScript-only navigation Google can't follow -->
<a href="javascript:void(0)" onclick="navigate('/about')">About</a>

<!-- ✅ GOOD: Real href -->
<a href="/about">About</a>
```

---

## 📱 "Document doesn't use legible font sizes"

```css
/* ❌ BAD: Too small on mobile */
p { font-size: 10px; }

/* ✅ GOOD: Minimum 16px for body text */
p { font-size: 16px; }
```

---

## 🤖 robots.txt and sitemap.xml

```
# public/robots.txt
User-agent: *
Allow: /
Sitemap: https://yourportfolio.com/sitemap.xml
```

```jsx
// app/sitemap.js (Next.js automatically generates sitemap)
export default function sitemap() {
  return [
    {
      url: 'https://yourportfolio.com',
      lastModified: new Date(),
    },
    {
      url: 'https://yourportfolio.com/projects',
      lastModified: new Date(),
    },
  ]
}
```

---

## ✅ SEO Checklist for Portfolios

- [ ] Unique `<title>` on every page
- [ ] Meta description on every page (150–160 chars)
- [ ] All links use real `href` values
- [ ] Images have descriptive `alt` text
- [ ] Body font size ≥ 16px
- [ ] `robots.txt` exists
- [ ] `sitemap.xml` exists
- [ ] Open Graph tags for social sharing

```jsx
// Open Graph tags (how your site looks when shared on LinkedIn/Twitter)
export const metadata = {
  openGraph: {
    title: 'John Doe — Frontend Developer',
    description: 'View my React and Next.js projects',
    images: [{ url: '/og-image.png', width: 1200, height: 630 }],
  },
}
```
