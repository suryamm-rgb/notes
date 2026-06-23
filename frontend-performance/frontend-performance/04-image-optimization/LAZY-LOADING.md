# 😴 Lazy Loading — Load Images Only When Visible

> **In plain English:** Do not load images the user has not scrolled to yet. Load them just before they scroll into view.

---

## What Is Lazy Loading?

Without lazy loading: ALL images download when the page loads, including images 2000px below the fold.

With lazy loading: Images download only when the user is about to see them.

**Impact:** A page with 20 images loads only the first 3-4 visible ones initially. The rest load on scroll. Much faster first load.

---

## next/image: Lazy by Default

The great news: `next/image` lazy loads all images automatically.

```jsx
import Image from 'next/image'

// This is automatically lazy loaded
<Image src="/project.jpg" alt="Project" width={800} height={450} />

// This is NOT lazy loaded (use for hero/above-fold images only)
<Image src="/hero.jpg" alt="Hero" width={1200} height={600} priority />
```

Rule: Add `priority` only to images visible immediately on page load (hero, header). Leave everything else without it.

---

## Native HTML Lazy Loading (without Next.js)

```html
<!-- Modern browsers support this natively -->
<img src="/project.jpg" alt="Project screenshot" loading="lazy" width="800" height="450" />

<!-- Do NOT add loading="lazy" to above-the-fold images -->
<img src="/hero.jpg" alt="Hero" loading="eager" width="1200" height="600" />
```

Browser support: 95%+

---

## Lazy Loading iframes (YouTube embeds, maps)

```html
<!-- YouTube embed: do not load until visible -->
<iframe
  src="https://www.youtube.com/embed/VIDEO_ID"
  loading="lazy"
  width="560"
  height="315"
  title="Project demo video"
></iframe>
```

Even better: use a thumbnail placeholder and load the iframe only on click:

```jsx
const [showVideo, setShowVideo] = useState(false)

return showVideo ? (
  <iframe src="https://youtube.com/embed/VIDEO_ID" width={560} height={315} />
) : (
  <button onClick={() => setShowVideo(true)}>
    <Image src="/video-thumbnail.jpg" alt="Play demo video" width={560} height={315} />
    <span>Play Demo</span>
  </button>
)
```

This saves users from loading the entire YouTube embed (hundreds of KB) unless they click.
