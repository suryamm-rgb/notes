# ✅ Best Practices Audits

---

## 🔒 "Uses HTTPS"
All modern hosting platforms (Vercel, Netlify, Cloudflare) enable HTTPS automatically. If you see this warning, switch hosting providers.

## 🖼️ "Images with incorrect aspect ratio"

```jsx
// ✅ Always match width/height to actual image proportions
// If your image is 1200x600, use those exact proportions
<Image src="/hero.jpg" alt="Hero" width={1200} height={600} />
// NOT width={1200} height={400} — that distorts the image
```

## 📜 "Browser errors logged to console"

Open DevTools Console (`F12` → Console). Fix any red errors you see.

## 🍪 "Uses deprecated APIs"

Lighthouse flags outdated JavaScript APIs. Check the warning detail for the specific API and look up the modern replacement on MDN.

## 📊 Best Practices Checklist

- [ ] Site uses HTTPS
- [ ] No console errors
- [ ] Images have correct aspect ratios
- [ ] No deprecated JavaScript APIs used
- [ ] Doctype declared (`<!DOCTYPE html>`)
- [ ] Character set declared (`<meta charset="UTF-8">`)
