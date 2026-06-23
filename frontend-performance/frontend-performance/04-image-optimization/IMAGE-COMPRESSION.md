# 🗜️ Image Compression — Shrink Images Before Uploading

> The most impactful thing you can do for performance: compress your images before putting them on your site.

---

## Target File Sizes

| Image Type | Maximum Size |
|------------|-------------|
| Hero / background | 200KB |
| Profile photo | 50KB |
| Project screenshot | 100KB |
| Thumbnail | 30KB |
| Icon / logo | Use SVG instead |

---

## Free Tools

### Squoosh (Best — Online, No Install)
1. Go to **https://squoosh.app**
2. Drag your image onto the page
3. On the right side, choose **WebP** format
4. Set quality to **80**
5. Compare before/after with the slider
6. Click **Download**

Example: 2.4MB JPEG → 180KB WebP (92% smaller, same visual quality)

### TinyPNG / TinyJPG
1. Go to **https://tinypng.com**
2. Upload your PNG or JPEG
3. Download compressed version

### SVGOMG (For SVG files)
1. Go to **https://svgomg.net**
2. Upload your SVG
3. Enable optimizations
4. Download — usually 30-50% smaller

---

## Automate Compression in Next.js Build

```bash
npm install --save-dev sharp
```

`sharp` is automatically used by `next/image` for server-side image optimization. When you use `<Image>`, Next.js uses sharp under the hood to compress and convert images.

---

## Choosing the Right Format

```
Photos and complex images → WebP
Simple graphics, logos    → SVG  
Screenshots               → WebP or PNG
Transparent backgrounds   → WebP (or PNG if compatibility needed)
Animations                → WebP (instead of GIF)
```

---

## Checklist Before Adding Any Image to Your Portfolio

- [ ] Is the file size under the target for its type?
- [ ] Is it in WebP format (not JPEG)?
- [ ] Does it have the correct dimensions (not 3x larger than displayed)?
- [ ] Does it have descriptive alt text?
- [ ] For hero images: added `priority` prop?
