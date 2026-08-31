---
name: brand-pwa-cross-promotion
description: Guidelines and patterns for implementing cross-promotional header banners, configuring PWA display modes, and ensuring brand icon consistency across Next.js projects.
---

# Brand & PWA Cross-Promotion Implementation Guidelines

This skill documents how to integrate cross-promotional banners, align branding assets, and configure PWA features correctly in Next.js multi-app ecosystems.

---

## 1. Cross-Promotional Header Banner

When adding banner cross-promotions to drive traffic to sister sites/applications, place the banner at the very top of the header/navigation component.

### Design Pattern (Tailwind CSS)
```tsx
export default function Header() {
  return (
    <header className="sticky top-0 z-40 border-b border-line bg-paper/85 backdrop-blur-md">
      {/* Sister Service Promotion Banner */}
      <div className="bg-sand/30 border-b border-line py-1.5 text-center text-[10px] sm:text-xs">
        <span className="font-semibold text-ink-soft">姉妹サービス: </span>
        <a 
          href="https://sister-app.vercel.app" 
          target="_blank" 
          rel="noopener noreferrer"
          className="font-bold text-ink hover:text-clay inline-flex items-center gap-0.5 transition-colors underline decoration-dotted"
        >
          ミセルリフォームはこちら 🎨 ➔
        </a>
      </div>
      
      {/* Navigation bar content */}
      <div className="mx-auto flex max-w-6xl items-center justify-between px-4 py-3 sm:px-6 sm:py-4">
        ...
      </div>
    </header>
  );
}
```

- **Requirements:**
  - Always use `target="_blank" rel="noopener noreferrer"` to open links in a new tab.
  - Keep styling lightweight and visually separated from the main header navigation (using subtle backgrounds like `bg-sand/30` or borders).
  - Use clear emojis (e.g. `🎨` or `🏠`) to draw eyes subtly without cluttering.

---

## 2. Branding Icons & PWA Consistency

To maintain unified branding across tabs and mobile/desktop installations:

### 1. Web Icon Updates
Replace default Next.js/React icons with custom brand logos in the following locations:
- `app/icon.png` (used by Next.js app router automatically for tab icon)
- `public/icon-192.png` (PWA standard icon)
- `public/icon-512.png` (PWA standard icon)

### 2. PWA Manifest (`public/manifest.json`)
Make sure the display mode matches requirements. Setting `"display": "browser"` prevents full-screen app wrapping, forcing it to open in standard web browser windows when installed.

```json
{
  "name": "WallAI",
  "short_name": "WallAI",
  "description": "AI-powered exterior wall painting simulator.",
  "start_url": "/",
  "display": "browser",
  "background_color": "#ffffff",
  "theme_color": "#111827",
  "orientation": "portrait"
}
```

---

## 3. Header & Footer Standard Items & Layout

To keep branding and regulatory pages consistent across all sites, implement the following header and footer items:

### 1. Header Layout
- **Brand Logo:** Left side. Next to it, the service title/slogan.
- **Navigation Links:** Menu items or call-to-actions (e.g. upload page, pricing).
- **Theme/PWA elements:** Right side. Keep user account buttons, or login menus if any.

### 2. Footer Layout & Standard Links
The footer should have a clean, light-colored background (e.g., `bg-slate-50` or similar paper tones) with high contrast text for links.

#### Mandatory Links:
- **特定商取引法に基づく表記 (Act on Specified Commercial Transactions):** Linked as a new tab (`/tokushoho` route).
- **利用規約 (Terms of Use):** Usually triggered as a modal window overlay or direct link.
- **プライバシーポリシー (Privacy Policy):** Modal overlay or direct link.
- **お問い合わせ (Contact/Support):** Contact form modal or direct path.
- **ブログ (Blog):** Links directly to `/blog`.

#### Design Pattern Example (Tailwind CSS):
```tsx
<footer className="bg-slate-50 border-t border-slate-100 py-8 px-6 text-center">
  <div className="max-w-6xl mx-auto flex flex-col md:flex-row justify-between items-center gap-4 text-xs text-slate-500 font-semibold">
    <div>
      © {new Date().getFullYear()} BrandName. All rights reserved.
    </div>
    <div className="flex flex-wrap justify-center gap-6">
      <button onClick={() => setModal('contact')} className="hover:text-indigo-600">お問い合わせ</button>
      <button onClick={() => setModal('terms')} className="hover:text-indigo-600">利用規約</button>
      <button onClick={() => setModal('privacy')} className="hover:text-indigo-600">プライバシーポリシー</button>
      <a href="/tokushoho" target="_blank" rel="noopener noreferrer" className="hover:text-indigo-600">特定商取引法に基づく表記</a>
      <Link href="/blog" className="hover:text-indigo-600">ブログ</Link>
    </div>
  </div>
</footer>
```
