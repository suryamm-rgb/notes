# 🖼️ next/image — The Right Way to Use Images in Next.js

> `next/image` is a built-in Next.js component that automatically optimizes images. Always use it instead of `<img>`.

---

## What next/image Does Automatically

- Converts images to WebP or AVIF
- Resizes images to the size you display them
- Lazy loads images by default
- Prevents layout shifts (reserves space before image loads)
- Serves images via CDN

---

## Basic Usage

```jsx
import Image from 'next/image'

// Local image (from /public folder)
<Image
  src="/hero-photo.jpg"
  alt="Portrait of John, frontend developer"
  width={600}
  height={400}
/>

// Remote image (from URL)
<Image
  src="https://example.com/photo.jpg"
  alt="Project screenshot"
  width={800}
  height={450}
/>
```

---

## Important Props

### priority (for above-the-fold images)
```jsx
// Add priority to your hero/main image — it loads immediately
<Image
  src="/hero.jpg"
  alt="Hero"
  width={1200}
  height={600}
  priority   // Without this, hero image lazy-loads (hurts LCP)
/>
```

### fill (for images that fill their container)
```jsx
// When you don't know exact dimensions
<div style={{ position: 'relative', width: '100%', height: '400px' }}>
  <Image
    src="/background.jpg"
    alt="Background"
    fill
    style={{ objectFit: 'cover' }}
  />
</div>
```

### sizes (for responsive images)
```jsx
// Tell the browser what size this image renders at different viewports
<Image
  src="/project-screenshot.png"
  alt="Project screenshot"
  width={800}
  height={450}
  sizes="(max-width: 768px) 100vw, (max-width: 1200px) 50vw, 800px"
  // On mobile: full width | On tablet: half width | On desktop: 800px
/>
```

### quality (default: 75)
```jsx
// Higher = better quality, larger file
// For hero images: 85-90
// For thumbnails: 70-75
<Image src="/hero.jpg" alt="Hero" width={1200} height={600} quality={85} />
```

---

## Configuring Remote Images

If loading images from external URLs, add the domain to next.config.js:

```javascript
// next.config.js
module.exports = {
  images: {
    remotePatterns: [
      {
        protocol: 'https',
        hostname: 'images.unsplash.com',
      },
      {
        protocol: 'https',
        hostname: 'github.com',
      },
    ],
  },
}
```

---

## Common Mistakes

```jsx
// MISTAKE 1: Forgetting priority on hero image (hurts LCP)
<Image src="/hero.jpg" alt="Hero" width={1200} height={600} />
// Fix: add priority prop

// MISTAKE 2: Wrong width/height ratio (image gets distorted)
<Image src="/photo-4x3.jpg" alt="Photo" width={400} height={400} />
// Fix: use actual aspect ratio (e.g., width={400} height={300} for 4:3)

// MISTAKE 3: Using <img> instead of <Image>
<img src="/photo.jpg" alt="Photo" />
// Fix: import Image from 'next/image' and use <Image>
```

---

## Portfolio Hero Example (Complete)

```jsx
import Image from 'next/image'

export default function Hero() {
  return (
    <section>
      <div style={{ position: 'relative', width: '100%', height: '500px' }}>
        <Image
          src="/profile-photo.jpg"
          alt="John Doe, Frontend Developer based in Bengaluru"
          fill
          style={{ objectFit: 'cover', objectPosition: 'top' }}
          priority
          quality={85}
          sizes="(max-width: 768px) 100vw, 50vw"
        />
      </div>
      <h1>John Doe</h1>
      <p>Frontend Developer</p>
    </section>
  )
}
```
