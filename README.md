# Diomika Storefront

Modern e-commerce platform for furniture business. Built for **performance**, **accessibility**, and **conversion**.

![Vite](https://img.shields.io/badge/vite-5.3-blue)
![Vue](https://img.shields.io/badge/vue-3.5-green)
![License](https://img.shields.io/badge/license-proprietary-red)

## Features

- **⚡ Performance First**: Vite build, minified + gzip, Core Web Vitals optimized
- **🔒 Security**: CSP, HSTS, X-Frame-Options, Permissions-Policy, Turnstile anti-spam
- ♿ **Accessibility**: ARIA labels, semantic HTML, keyboard navigation, reduced-motion support
- 📊 **Analytics**: PostHog integration with custom conversion events (quote_submitted, contact_message_sent)
- 🖼️ **SEO**: Prerendered pages per category/model, dynamic sitemap, structured robots.txt
- 📱 **Responsive**: Mobile-first design, tested from 375px to desktop
- 🚀 **Fast Deployments**: GitHub Actions CI/CD, Cloudflare Pages CDN, auto-rollback on failure
- 💾 **State**: Cart persistence, quote draft recovery via localStorage

## Tech Stack

| Layer | Tech |
|-------|------|
| **Frontend** | Vue 3.5 + Vue Router 5 + Vite 5.3 |
| **Styling** | Custom CSS with design system (Archivo font) |
| **API Client** | Supabase JS with custom retry logic |
| **Forms** | Cloudflare Turnstile (anti-spam) + idempotency keys |
| **Hosting** | Cloudflare Pages (edge cache, functions, WAF) |
| **Analytics** | PostHog (lazy-loaded, CSP-compliant) |

## Project Structure

```
frontend-web/
├── src/
│   ├── components/     # Vue components
│   ├── views/          # Page components (CartView, ContactView, etc)
│   ├── composables/    # Vue 3 composables (useCart, useTurnstile)
│   ├── lib/            # Utilities (api client, PostHog, constants)
│   └── main.css        # Design system
├── functions/
│   ├── api/[[path]].js # Cloudflare Functions router + security headers
│   └── _middleware.js  # CSP, HSTS, Permissions-Policy
├── public/
│   ├── 404.html        # Custom error page
│   ├── 500.html        # Server error page
│   └── robots.txt      # SEO rules
└── vite.config.js      # Prerender config
```

## Getting Started

### Prerequisites
- Node.js 18+
- npm 9+

### Install

```bash
npm install
```

### Development

```bash
npm run dev
```

Runs at `http://localhost:5173` with hot reload.

### Build

```bash
npm run build
```

Outputs to `dist/` with:
- Minified JS/CSS
- Prerendered pages per category/model
- Lazy-loaded async chunks

### Preview

```bash
npm run preview
```

## Environment Variables

Create `.env.local`:

```env
VITE_API_URL=https://api.diomika.com
VITE_SUPABASE_URL=https://your-supabase-url.supabase.co
VITE_SUPABASE_KEY=your-public-key
VITE_POSTHOG_KEY=your-posthog-key
VITE_TURNSTILE_SITE_KEY=your-turnstile-key
```

See `.env.example` for all available variables.

## API Integration

Cart → Quote Request flow:

1. **User adds items** → stored in cart (localStorage + sessionStorage for sync)
2. **Submit quote** → POST `/orcamentos` with idempotency key
3. **Success** → PostHog `quote_submitted` event, cart clears, confirmation shown

Contact form similarly uses Turnstile + idempotency for anti-spam.

## Accessibility

- 26+ ARIA labels for form fields and interactive elements
- Semantic HTML (`<button>`, `<input>`, proper form structure)
- Keyboard navigation: Tab, Enter, Escape all functional
- Reduced motion: CSS respects `prefers-reduced-motion`
- Color contrast: WCAG AA compliant

## Performance

- **Vite**: Code splitting, tree-shaking, CSS minification
- **Cloudflare**: Gzip compression, edge caching (HTML: 5min, assets: 1 year)
- **Images**: Lazy-load with `loading="lazy"` + dimensions for CLS prevention
- **PostHog**: Async-loaded, won't block page render
- **Core Web Vitals**: LCP <2.5s, FID <100ms, CLS <0.1 (target)

## Security

- **CSP**: Default-src 'self', script-src posthog.com/cloudflareinsights.com, no unsafe-inline
- **HSTS**: Strict-Transport-Security: max-age=31536000 (1 year)
- **Clickjacking**: X-Frame-Options: DENY
- **MIME sniffing**: X-Content-Type-Options: nosniff
- **Anti-spam**: Cloudflare Turnstile + idempotency keys
- **WAF**: Cloudflare Pages with x-diomika-desktop header gate for backoffice

## Deployment

Automatic on push to `main`:

1. GitHub Actions runs gitleaks, npm audit, Tripy
2. Vite build + prerendering
3. Deploy to Cloudflare Pages
4. Edge cache invalidation via catalogue_version bump
5. Auto-rollback if health checks fail

## License

Proprietary. See LICENSE file.

---

**Built by Tomás with ❤️ for precision and speed.**
