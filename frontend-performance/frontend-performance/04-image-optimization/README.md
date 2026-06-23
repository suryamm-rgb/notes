# 🖼️ Image Optimization — Why Images Kill Performance

> Images are the single biggest cause of slow websites. On average, images make up 50-75% of a webpage's total size.

---

## The Problem

A typical unoptimized portfolio:
- Hero image: 3.2MB JPEG (actual display size: 400KB needed)
- Project screenshots: 5x 1.5MB = 7.5MB (actual needed: ~300KB each)
- Profile photo: 800KB (actual needed: ~50KB)
- **Total: ~11MB** for images alone

After optimization:
- **Total: ~800KB** — a 93% reduction, pages load 10x faster

---

## The 3 Image Optimization Techniques

| Technique | File | What It Does |
|-----------|------|-------------|
| next/image | `NEXT-IMAGE.md` | Automatic resizing, format conversion, lazy loading |
| Lazy Loading | `LAZY-LOADING.md` | Load images only when visible |
| Compression | `IMAGE-COMPRESSION.md` | Reduce file size before uploading |

---

## Quick Wins Right Now

1. **Convert all images to WebP** → https://squoosh.app
2. **Add `priority` to your hero image** → `<Image priority />`
3. **Add `alt` to every image** → accessibility + SEO
4. **Never use images > 200KB** on a portfolio

---

## Image Format Guide

| Format | Best For | Browser Support |
|--------|---------|----------------|
| **WebP** | Photos, general use | 97%+ of browsers |
| **AVIF** | Maximum compression | 90%+ of browsers |
| **SVG** | Icons, logos, illustrations | 100% |
| **PNG** | Screenshots with transparency | 100% |
| **JPEG** | Avoid — use WebP instead | 100% |
| **GIF** | Avoid — use WebP video instead | 100% |

---

*Start with: `NEXT-IMAGE.md` if you use Next.js, or `IMAGE-COMPRESSION.md` for any project.*
