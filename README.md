# DCG Land Management website

Static one-page site built with plain HTML, CSS and JavaScript so it is inexpensive to host and easy to maintain.

## Files
- `index.html` — page content + SEO metadata/schema
- `styles.css` — all layout and visual styling
- `script.js` — mobile menu, before/after slider, reveal animation and estimate email form
- `assets/` — optimized client photos and logo
- `robots.txt` / `sitemap.xml` — basic search engine crawling support

## Before publishing
1. Confirm the final domain. This build currently uses `https://dcgland.com/` in canonical, Open Graph, schema, robots and sitemap tags because the business email uses that domain.
2. The estimate form intentionally opens the visitor's email app addressed to `craig@dcgland.com`; no third-party form service or backend is required. If you later want in-page submissions, connect Formspree, Netlify Forms, Basin, or another endpoint.
3. Confirm that `72 Northwood Circle, Oakman, AL 35579` should be publicly displayed. If it is a private/home address, remove it from the footer and structured data and use only Oakman, AL.
4. Confirm the service-area list and business insurance wording before launch.

## Local preview
Open `index.html` directly, or run a simple local server from this folder:

```bash
python3 -m http.server 5173
```

Then visit `http://localhost:5173`.
