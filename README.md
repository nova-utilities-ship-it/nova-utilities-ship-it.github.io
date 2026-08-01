# Nova Utilities

The official website for the **Nova Utilities** ecosystem — a collection of modern, offline-first Android utility applications focused on privacy, performance and simplicity.

Live site: **https://nova-utilities-ship-it.github.io/**

Built with plain **HTML5, CSS3 and vanilla JavaScript** — no frameworks, no build step, no backend. Fully static and optimized for GitHub Pages.

---

## Table of Contents

- [Project Overview](#project-overview)
- [Tech Stack](#tech-stack)
- [Folder Structure](#folder-structure)
- [Local Development](#local-development)
- [Deploying to GitHub Pages](#deploying-to-github-pages)
- [Customization](#customization)
  - [Changing Colors / Theme](#changing-colors--theme)
  - [Adding a New Application](#adding-a-new-application)
  - [Updating the Changelog](#updating-the-changelog)
  - [Editing SEO Metadata](#editing-seo-metadata)
- [Pages](#pages)
- [License](#license)

---

## Project Overview

This repository contains the complete source for the Nova Utilities marketing/informational website: home page, application catalog, about, support (FAQ), contact, changelog, privacy policy, terms & conditions, and a custom 404 page.

Design goals:

- **Lightweight** — no JS or CSS frameworks, a single stylesheet, a single script file, and an inline SVG icon sprite (no image requests).
- **Responsive** — mobile, tablet and desktop breakpoints.
- **Accessible** — semantic HTML, skip link, keyboard-friendly navigation, native `<details>` for FAQs/accordions.
- **SEO-friendly** — per-page meta tags, Open Graph, Twitter Card tags, `sitemap.xml`, `robots.txt`, canonical URLs.
- **Themeable** — light and dark themes via CSS custom properties, persisted with `localStorage` and respecting the OS preference by default.

## Tech Stack

- HTML5
- CSS3 (custom properties, Grid, Flexbox, `backdrop-filter` glassmorphism)
- Vanilla JavaScript (ES5-compatible, no build tooling required)
- [Inter](https://fonts.google.com/specimen/Inter) via Google Fonts (optional — remove the `<link>` tags in every `<head>` to go fully offline/self-hosted)

## Folder Structure

```
nova-utilities-ship-it.github.io/
├── index.html                 # Home
├── apps.html                  # Applications
├── about.html                 # About Nova Utilities
├── support.html                # Support / FAQ
├── contact.html                # Contact
├── changelog.html              # Release Notes
├── privacy.html                 # Privacy Policy
├── terms.html                   # Terms & Conditions
├── 404.html                     # Custom 404 page
├── assets/
│   ├── css/
│   │   └── style.css          # Single global stylesheet (design system + all pages)
│   ├── js/
│   │   └── main.js            # Theme toggle, mobile nav, scroll reveal, etc.
│   ├── icons/
│   │   └── sprite.svg         # Shared inline SVG icon sprite (<use> references)
│   ├── favicon/
│   │   └── favicon.svg        # Brand mark / favicon (SVG, scalable)
│   └── images/                # Reserved for any raster images you add later
├── manifest.webmanifest        # Web app manifest (PWA metadata)
├── robots.txt
├── sitemap.xml
├── .nojekyll                    # Disables Jekyll processing on GitHub Pages
├── .github/
│   └── workflows/
│       └── deploy.yml          # GitHub Actions workflow to auto-deploy on push to main
├── README.md
└── LICENSE
```

## Local Development

No build step is required. Any static file server works:

```bash
# Python
python -m http.server 8080

# Node (npx, no install)
npx serve .
```

Then open `http://localhost:8080`.

You can also open `index.html` directly in a browser, but serving over HTTP is recommended so relative paths and the manifest behave exactly as they will in production.

## Deploying to GitHub Pages

This repository is set up as a **GitHub Organization Pages site** (`nova-utilities-ship-it.github.io`), which publishes automatically at the repository root — `https://nova-utilities-ship-it.github.io/`.

Deployment is automated via [`.github/workflows/deploy.yml`](.github/workflows/deploy.yml):

1. Push to the `main` branch.
2. GitHub Actions checks out the repo, uploads it as a Pages artifact, and deploys it — no build step needed since the site is already static.
3. In your repository settings, go to **Settings → Pages** and set the **Source** to **GitHub Actions** (one-time setup).

That's it — every push to `main` redeploys the live site.

> If you later move to a custom domain, add a `CNAME` file at the repository root containing your domain, update DNS, and update the URLs described below in [Editing SEO Metadata](#editing-seo-metadata).

## Customization

### Changing Colors / Theme

All colors are defined as CSS custom properties at the top of [`assets/css/style.css`](assets/css/style.css):

```css
:root {
  --nova-blue: #2563eb;
  --nova-blue-light: #3b82f6;
  --nova-indigo: #6366f1;
  --nova-purple: #7c3aed;
  --nova-purple-light: #a855f7;

  --gradient-primary: linear-gradient(135deg, var(--nova-blue) 0%, var(--nova-indigo) 45%, var(--nova-purple) 100%);
}
```

Light and dark theme surface/text colors are defined separately under `[data-theme="light"]` and `[data-theme="dark"]` in the same file. Edit the values there to change background, text, border and surface colors per theme — every component (buttons, cards, header, footer) references these variables, so a single edit updates the whole site.

### Adding a New Application

1. Open [`apps.html`](apps.html).
2. Duplicate one of the `<div class="card app-card reveal">...</div>` blocks.
3. Update the icon (`<use href="assets/icons/sprite.svg#icon-...">` — pick an existing icon from `assets/icons/sprite.svg`, or add a new `<symbol>` there), the app name, description, status badge (`badge-available` or `badge-soon`), and the bullet list inside `.app-more-body`.
4. Optionally add a matching preview card to the "Lineup" section on [`index.html`](index.html).
5. When the app status changes from "Coming Soon" to "Available", swap the badge class from `badge-soon` to `badge-available` and update the label text.

### Updating the Changelog

Open [`changelog.html`](changelog.html) and duplicate a `.timeline-item` block inside `.timeline`:

```html
<div class="timeline-item reveal">
  <span class="timeline-dot"><svg class="icon"><use href="assets/icons/sprite.svg#icon-zap"></use></svg></span>
  <div class="timeline-card">
    <span class="timeline-version">Version 1.1 <span class="badge badge-available">Update</span></span>
    <span class="timeline-date">Month Day, Year</span>
    <ul>
      <li>Describe what changed here</li>
    </ul>
  </div>
</div>
```

Place newer entries **above** older ones so the timeline reads newest-first.

### Editing SEO Metadata

Every page's `<head>` includes `<title>`, meta description, canonical URL, Open Graph and Twitter Card tags. If you change the domain (e.g. add a custom domain), update:

- `<link rel="canonical">` and `og:url` / `twitter:*` in every HTML file's `<head>`
- `sitemap.xml` (all `<loc>` entries)
- `robots.txt` (`Sitemap:` line)
- `manifest.webmanifest` (`start_url`, `scope`, `id`)

The current social preview image (`og:image` / `twitter:image`) points to the SVG favicon as a lightweight default. For best compatibility with platforms that don't render SVG previews (e.g. some social media crawlers), consider generating a dedicated 1200×630 PNG social card and updating those meta tags to point to it.

## Pages

| Route | File | Description |
|---|---|---|
| `/` | `index.html` | Home |
| `/apps` | `apps.html` | Applications |
| `/about` | `about.html` | About Nova Utilities |
| `/support` | `support.html` | Support & FAQ |
| `/contact` | `contact.html` | Contact |
| `/changelog` | `changelog.html` | Release Notes |
| `/privacy` | `privacy.html` | Privacy Policy |
| `/terms` | `terms.html` | Terms & Conditions |
| `/404` | `404.html` | Custom 404 page |

## License

Released under the [MIT License](LICENSE).
