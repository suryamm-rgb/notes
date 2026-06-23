# ♿ Accessibility Audits — Fix Every Warning

> **Why this matters:** 1 in 5 people has a disability. Accessibility also improves SEO and usability for everyone.

---

## 🖼️ "Image elements do not have [alt] attributes"

Every `<img>` must have an `alt` attribute.

```html
<!-- ❌ BAD -->
<img src="/photo.jpg" />

<!-- ✅ GOOD: Descriptive alt text -->
<img src="/photo.jpg" alt="Screenshot of my weather app showing a 5-day forecast" />

<!-- ✅ GOOD: Decorative image (empty alt = screen reader skips it) -->
<img src="/divider-line.svg" alt="" />
```

**Rule of thumb:**
- Informative images → describe what you see
- Decorative images → use `alt=""`
- Linked images → describe the destination ("Link to GitHub repository")

---

## 🔘 "Buttons do not have an accessible name"

```html
<!-- ❌ BAD: Screen reader says "button" with no context -->
<button><svg>...</svg></button>

<!-- ✅ GOOD: aria-label describes the action -->
<button aria-label="Close menu"><svg>...</svg></button>

<!-- ✅ ALSO GOOD: Visually hidden text -->
<button>
  <svg aria-hidden="true">...</svg>
  <span class="sr-only">Close menu</span>
</button>
```

```css
/* Screen-reader-only utility class */
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border-width: 0;
}
```

---

## 🔗 "Links do not have a discernible name"

```html
<!-- ❌ BAD: "Read more" says nothing out of context -->
<a href="/project-1">Read more</a>

<!-- ✅ GOOD: Descriptive link text -->
<a href="/project-1">Read more about my Weather App project</a>

<!-- ✅ GOOD: aria-label for icon links -->
<a href="https://github.com/you" aria-label="My GitHub profile">
  <svg aria-hidden="true">...</svg>
</a>
```

---

## 🎨 "Background and foreground colors do not have sufficient contrast"

Text must have enough contrast against its background.

| Text Size | Minimum Ratio | Enhanced Ratio |
|-----------|--------------|----------------|
| Normal text (< 18px) | 4.5:1 | 7:1 |
| Large text (≥ 18px or bold ≥ 14px) | 3:1 | 4.5:1 |

**Check contrast:** https://webaim.org/resources/contrastchecker/

```css
/* ❌ BAD: Light gray on white — fails contrast */
.subtitle { color: #aaaaaa; background: #ffffff; } /* 2.32:1 ratio */

/* ✅ GOOD: Darker gray passes */
.subtitle { color: #767676; background: #ffffff; } /* 4.54:1 ratio */
```

---

## 📋 "Form elements do not have associated labels"

```html
<!-- ❌ BAD: Input with no label -->
<input type="email" placeholder="Enter your email" />

<!-- ✅ GOOD: Label connected via htmlFor/id -->
<label htmlFor="email">Email address</label>
<input type="email" id="email" placeholder="Enter your email" />

<!-- ✅ ALSO GOOD: aria-label -->
<input type="email" aria-label="Email address" placeholder="Enter your email" />
```

---

## ⌨️ "Interactive controls are not keyboard focusable"

All interactive elements must be usable with a keyboard (Tab key navigation).

```jsx
// ❌ BAD: div is not keyboard accessible
<div onClick={handleClick}>Click me</div>

// ✅ GOOD: Use semantic HTML
<button onClick={handleClick}>Click me</button>

// ✅ GOOD: If you must use a div
<div
  role="button"
  tabIndex={0}
  onClick={handleClick}
  onKeyDown={(e) => e.key === 'Enter' && handleClick()}
>
  Click me
</div>
```

---

## 📋 Accessibility Checklist for Portfolios

- [ ] All images have `alt` text
- [ ] All buttons have accessible names
- [ ] All links have descriptive text
- [ ] Color contrast passes 4.5:1 for normal text
- [ ] All form inputs have labels
- [ ] Page has a `<main>` landmark
- [ ] Headings are in logical order (h1 → h2 → h3)
- [ ] Page has a `lang` attribute on `<html>`

```html
<!-- ✅ Required: lang attribute -->
<html lang="en">
```

---

*Lighthouse Accessibility target: **100/100** — aim for perfect.*
