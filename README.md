# Corona-Lifestyle-Landing

Single-screen brand landing built on one line of copy — *living is a sensory experience* — broken mid-word across a two-colour divide.

## Description

A lifestyle brand page with almost no content: a mark, a headline and a hashtag. Everything rests on how the headline is set. The phrase is split mid-word — `LIVING IS A SEN` / `SORY EXPERIENCE` — so the break is a deliberate typographic device rather than a wrap.

The divide is the point. Below 768px it runs horizontally and the phrase breaks across two lines; from 768px up it runs vertically and the two halves sit on a shared baseline either side of the centre line, so the word "sensory" is cut by the colour boundary itself. That boundary is not pinned to a fixed percentage — it is drawn by the elements, so it always lands exactly where the word breaks, at any viewport size.

The split makes the markup announce "sen" and "sory" as two words, so the `h1` carries an `aria-label` with the whole phrase. The visual break survives; the sentence does too.

Type is Cinzel, a serif derived from Roman inscriptions, which is also what the emblem is lettered in. It is the only webfont; everything else is set in the system UI stack, which costs nothing to load.

The project contains no JavaScript, no build step and no dependencies.

## Tech stack

| Layer | Technology | Detail |
|---|---|---|
| Markup | HTML5 | `index.html` and `404.html`, no templating |
| Styling | CSS3 | Custom properties, Grid with `subgrid`, mobile-first `min-width` queries |
| Typography | Cinzel variable | Self-hosted WOFF2, Latin subset, weights 400–900, 22.7 KB |
| Typography | System UI stack | `system-ui` and platform fallbacks, 0 bytes |
| Images | WebP | Emblem and artwork; PNG only for the favicon and the social card |
| Scripting | None | The project contains no JavaScript files |
| Build | None | The repository root is the deployable artifact |

First load is 227 KB over 8 requests.

## Project structure

```
.
├── index.html                        # The landing. Single screen, no scroll.
├── 404.html                          # Same visual system, links back to the root
├── robots.txt                        # Allow all + sitemap pointer
├── sitemap.xml                       # One URL; the site is one page
├── assets/
│   ├── css/
│   │   ├── base.css                  # Tokens, @font-face, reset, base type
│   │   ├── layout.css                # Page shell, header, stage grid, footer
│   │   └── components.css            # Brand, headline, artwork, links, button
│   ├── fonts/
│   │   ├── cinzel-variable.woff2     # Subset to Latin, wght 400–900
│   │   └── ofl.txt                   # SIL Open Font License for Cinzel
│   └── img/
│       ├── logo/
│       │   ├── corona-emblem.webp    # 256px, the on-page mark
│       │   ├── favicon.png           # 96px
│       │   └── apple-touch-icon.png  # 180px
│       └── content/
│           ├── chrome-abstract.webp  # 800px, the one photographic asset
│           └── og-card.png           # 1200x630 social card
└── docs/
    ├── auditoria.md                  # State of the project before the rebuild
    └── cambios.md                    # What changed, by phase
```

## Running locally

The site is static, so any HTTP server works:

```bash
npx serve@14 .
```

Then open the address the server prints.

Opening `index.html` straight off disk also renders correctly, with one caveat: Chrome treats `file://` documents as an opaque origin and blocks the `@font-face` request, so the headline falls back to the declared serif (Georgia, then Times New Roman). Serve over HTTP to see Cinzel.

## Deployment

Static hosting, no build command and no output directory — upload the repository root as it is. Canonical URL, Open Graph tags and `sitemap.xml` all point at `https://pablowib.github.io/Corona-Lifestyle-Landing/`; change those four references if the domain changes.

Point the host's 404 handler at `404.html`. On GitHub Pages and Netlify a root-level `404.html` is picked up automatically.

## Fonts and licensing

Cinzel is licensed under the SIL Open Font License; `assets/fonts/ofl.txt` is the licence as distributed and must travel with the font file if it is redistributed. The shipped `.woff2` is a Latin subset of the upstream variable font, generated with `fonttools`.

## Image credits

`chrome-abstract.webp` is not an original asset. The source file carries the watermark *by @songsandthespirits* and is credited in the page footer. Confirm the usage rights before publishing this page commercially.

## Author

**Pablo Nieto Pérez** — [wib.digital](https://wib.digital)
GitHub: [@pabloWIB](https://github.com/pabloWIB)

## Hire me

I build **custom internal tools, CRMs and dashboards** for small teams, and
**conversion-focused websites** for businesses.

- [Custom internal tool, CRM or dashboard](https://www.fiverr.com/pablonietop/build-a-custom-internal-app-for-your-business) — from $45
- [Conversion-focused website](https://www.fiverr.com/pablonietop/convert-your-landing-page-design-to-code) — from $80
- [All my services on Fiverr](https://www.fiverr.com/pablonietop)
- [wib.digital](https://wib.digital)
