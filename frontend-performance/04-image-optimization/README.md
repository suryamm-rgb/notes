# 04 — Image Optimization

**Images are usually the #1 reason sites are slow.** A single unoptimized phone photo can be 5 MB. Compare that to ~150 KB for all your JavaScript. One bad image = 30× more data than your entire app.

Good news: image optimization is the **easiest** performance win. You can often cut page weight by 80% in 10 minutes.

---

## The 3 rules of image optimization

1. **Right size** — don't serve a 4000×3000 image into a 400×300 box
2. **Right format** — use WebP/AVIF, not PNG/JPG
3. **Right loading strategy** — lazy-load below-the-fold images

---

## 1. Use the right format

| Format | Best for | Notes |
|--------|----------|-------|
| **AVIF** | Photos | Smallest size, modern browsers (~95% support) |
| **WebP** | Photos, screenshots | 25–35% smaller than JPEG, near-universal support |
| **JPEG** | Photos (legacy) | Use only as a fallback |
| **PNG** | Logos, icons with transparency | Use SVG instead when possible |
| **SVG** | Logos, icons, simple graphics | Tiny, scales infinitely |
| **GIF** | ❌ Avoid | Use MP4/WebM video instead |

> **Rule of thumb:** WebP for photos, SVG for icons/logos.

---

## 2. Compress your images

### Tool 1: Squoosh (easiest, in browser)
<https://squoosh.app/> — drag in an image, choose WebP, slide quality to ~75, download. Done.

### Tool 2: TinyPNG / TinyJPG
<https://tinypng.com/> — drag-and-drop bulk compressor.

### Tool 3: CLI (for batch processing)
```bash
# Install sharp-cli
npm install -g sharp-cli

# Convert and resize
sharp -i photo.jpg -o photo.webp resize 1600
```

### Tool 4: Build-time (automatic)
For Vite, use `vite-imagetools`:
```ts
import hero from './hero.jpg?w=1600&format=webp';
```

---

## 3. Use `next/image` (Next.js)

If you're using **Next.js**, the `<Image>` component automatically:
- Resizes images for each device size
- Converts to WebP/AVIF
- Lazy-loads below-the-fold images
- Reserves space (no layout shift)
- Adds blur placeholders

```tsx
import Image from 'next/image';

<Image
  src="/hero.jpg"
  alt="Hero"
  width={1600}
  height={900}
  priority   // for above-the-fold images (LCP)
/>
```

**Always set `priority` on your hero/LCP image.** This preloads it for fast LCP.

---

## 4. Lazy loading (don't load offscreen images)

Native HTML lazy loading is supported in all modern browsers:

```html
<img src="/photo.jpg" alt="..." loading="lazy" width="800" height="600" />
```

**Rules:**
- ✅ Lazy-load images **below the fold** (need to scroll to see)
- ❌ DON'T lazy-load **above-the-fold** images (especially the LCP image) — it delays them
- ✅ Always include `width` and `height` to prevent layout shift (CLS)

For background images via CSS, use `loading="lazy"` on a hidden `<img>` to control when the browser fetches.

---

## 5. Responsive images (`srcset`)

Don't send a 2000px image to a 400px phone screen:

```html
<img
  src="/photo-800.webp"
  srcset="/photo-400.webp 400w,
          /photo-800.webp 800w,
          /photo-1600.webp 1600w"
  sizes="(max-width: 600px) 400px,
         (max-width: 1200px) 800px,
         1600px"
  alt="..."
  width="800" height="600"
/>
```

The browser picks the smallest image that fits the user's screen. Massive bandwidth savings on mobile.

`next/image` and `vite-imagetools` generate `srcset` automatically.

---

## 6. Preload the LCP image

If your hero image is the biggest visible element (it usually is), preload it:

```html
<link rel="preload" as="image" href="/hero.webp" fetchpriority="high">
```

This tells the browser "start downloading this immediately, before any JS or CSS."

---

## 7. Use a CDN / image service

Services that resize and serve images on demand:
- **Cloudinary** (free tier)
- **imgix**
- **Cloudflare Images**
- **Vercel** (built-in with Next.js)
- **Supabase Storage** with image transformations

Upload one big image, get any size/format on demand via URL parameters.

---

## ✅ Beginner checklist

For every image on your site:

- [ ] Resized to a sensible max width (1600–2000px for hero, smaller elsewhere)
- [ ] Converted to **WebP** or **AVIF**
- [ ] Has `width` and `height` attributes (prevents CLS)
- [ ] Has descriptive `alt` text (accessibility + SEO)
- [ ] `loading="lazy"` if below the fold
- [ ] `fetchpriority="high"` if it's the hero/LCP image
- [ ] Compressed to ~75% quality

Do this for your portfolio's hero image and you'll see an instant Lighthouse jump.

**Next:** `05-react-performance/README.md` — make React itself faster.
