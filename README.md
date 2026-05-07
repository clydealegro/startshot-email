# Starshot UX Email Newsletter

HTML email implementation of the [Starshot UX Email Newsletter](https://www.figma.com/design/yLguowSDr11irG75AKEmpn/Untitled?node-id=0-5) Figma design.

## Structure

```
starshot-ux/
├── index.html
├── assets/
│   ├── hero-image.png
│   ├── header-divider.png
│   ├── article-1.png
│   ├── article-2.png
│   ├── divider.png
│   ├── footer-divider.png
│   ├── logo.png
│   ├── arrow-right.png
│   └── social/
│       ├── icon-1.png
│       ├── icon-2.png
│       └── icon-3.png
└── README.md
```

## Decisions

- **Hybrid table layout** — Tables handle macro structure (full-width sections, 2-column rows); inline CSS handles all typography and spacing. This is the most broadly compatible approach across Outlook, Gmail, and Apple Mail.
- **`<style>` block retained for media queries** — `@media` rules cannot be inlined. The style block is limited to CSS reset, dark mode overrides, and mobile breakpoints. All presentational CSS is inline.
- **Google Fonts `<link>` in `<head>`** — Used for Plus Jakarta Sans as specified. Arial is the declared fallback stack for clients that strip web font links (Outlook, Gmail Android).
- **@2x PNG assets** — All images exported from Figma at 2× and rendered at 1× display size so they appear sharp on retina/HiDPI screens without any code changes.
- **CSS gradient hero background** — The hero section uses `background: linear-gradient(170deg, #eeecf8 0%, #f4f2fb 100%)` inline with `bgcolor="#eeecf8"` as an Outlook attribute fallback. No background image is needed.
- **Solid VML CTA button** — Outlook's Word rendering engine ignores CSS backgrounds on `<a>` tags. A `<v:roundrect fillcolor="#160f2e">` inside `<!--[if mso]>` conditionals renders the button natively in Outlook without requiring gradient fill syntax.
- **`direction:rtl` for Article 2** — Figma shows text-left/image-right on desktop and image-first when stacked on mobile. Setting `direction:rtl` on the inner table reverses visual column order without duplicating content, while restoring `direction:ltr` on each cell keeps text readable.
- **Dark mode belt-and-suspenders** — `<meta name="color-scheme" content="light">` signals light-only intent to supporting clients; `@media (prefers-color-scheme: dark)` overrides then force white backgrounds and black text as a second layer of protection. The hero `<td>` gets an explicit `!important` lavender override so it stays lavender rather than inverting to dark.

## Assumptions

- CTA link (`https://www.starshotsoftware.com/post/top-6-ux-design-mistakes-in-healthcare-apps-and-how-to-fix-them`), Article 1 ("Read More" → `https://starshotsoftware.com/post/why-does-healthcare-needs-it-outsourcing`), and Article 2 ("Learn the Difference" → `https://starshotsoftware.com/post/top-qualities-for-ux-designers`) links provided by the client.
- Footer copyright year set to 2026 to match the current year.
- `border-radius: 8px` applied to all images (hero and article) based on visual inspection of the Figma design.
- Divider images (header, section, footer) are rendered at `height: 1px` matching their exported pixel height.
- Hero image background colour `#24dbc0` applied as a CSS `background-color` fallback for the image cell.

## Limitations

| Item | Detail |
|------|--------|
| CSS gradient hero | Outlook renders the solid `bgcolor="#eeecf8"` attribute fallback — no gradient. Visual difference is minor (light lavender solid vs. subtle lavender gradient). |
| Google Fonts | Falls back to Arial in Outlook and Gmail Android — web fonts are stripped by those clients. |
| `border-radius` on images | Ignored by Outlook; article and hero images appear with square corners in Outlook only. |
| `direction:rtl` (Article 2) | Supported in all tested modern clients. Outlook renders correctly via its table layout engine. |
| Dark mode | Apple Mail and Outlook 2019+ honour `prefers-color-scheme`. Gmail web ignores it and always renders in light mode — which is the intended appearance anyway. |
| Asset paths | Relative paths work for local preview. Replace with absolute CDN URLs before sending via an ESP. |

## Production

Before sending, replace relative asset paths in `index.html` with absolute CDN URLs (e.g. `https://cdn.yourdomain.com/starshot-ux/assets/hero-image.png`). Email clients cannot load relative paths.
