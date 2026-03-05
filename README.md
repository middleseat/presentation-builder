# Middle Seat Presentation Builder

A Claude Code skill for creating beautiful, on-brand HTML presentations in Middle Seat style. Forked from [zarazhangrui/frontend-slides](https://github.com/zarazhangrui/frontend-slides) and fully adapted for the Middle Seat brand system.

---

## What It Does

Generates zero-dependency, single-file HTML presentations using Middle Seat brand typography, colors, and layout — Cardo headings, Proxima Nova body text, Antarctican Mono labels, and the full pistachio/lavender/slate color palette. Decks run entirely in the browser with no npm, no build tools, and no external dependencies (except font CDN links).

**Four MS-exclusive visual presets:**

| Preset | Mood | Best For |
|--------|------|----------|
| MS Classic | Professional, editorial | Internal briefings, status updates |
| MS Bold | High-impact, dark | Campaign pitches, fundraising asks |
| MS Data | Analytics-forward | Polling reports, dashboards |
| MS Proposal | Warm, relationship-first | New business, client pitches |

---

## Prerequisites

You need:
- [Claude Code](https://claude.ai/code)
- Middle Seat brand assets at `~/middleseat/brand_assets/` (fonts + logos)
- An internet connection (for Typekit/Google Fonts CDN on generated slides)

Optional for PPT conversion:
```bash
pip install python-pptx
```

---

## Usage

In a Claude Code session, type:

```
/presentation-builder
```

Claude will guide you through:
1. **Content discovery** — Purpose, length, content readiness, editing preferences
2. **Style selection** — 3 visual previews based on your chosen mood, or pick a preset directly
3. **Generation** — Full branded HTML deck with embedded brand fonts and inlined MS logo
4. **Delivery** — Opens in your browser, ready to present

---

## File Structure

```
presentation-builder/
├── SKILL.md               # Claude Code skill — main workflow
├── MS_BRAND.md            # Middle Seat brand reference (colors, fonts, logo, components)
├── STYLE_PRESETS.md       # 4 MS visual presets with full CSS specs
├── viewport-base.css      # Mandatory responsive CSS (included in all generated decks)
├── html-template.md       # HTML architecture and JS patterns
├── animation-patterns.md  # CSS/JS animation reference
├── scripts/
│   └── extract-pptx.py   # PowerPoint content extraction
└── demo/
    └── middle-seat-capabilities.html  # Sample MS Classic deck
```

---

## Brand Assets Used

The skill reads these files at generation time:

| Asset | Path | How Used |
|-------|------|----------|
| Antarctican Mono font | `~/middleseat/brand_assets/fonts/AntarcticanMono-Medium.ttf` | Base64-embedded in every deck |
| Horizontal logo | `~/middleseat/brand_assets/logos/MS-horizontal.svg` | Inlined SVG in header bars and covers |
| Proxima Nova | Adobe Typekit CDN | Loaded via `<link>` in `<head>` |
| Cardo | Google Fonts CDN | Loaded via `<link>` in `<head>` |

---

## Demo Deck

See [`demo/middle-seat-capabilities.html`](demo/middle-seat-capabilities.html) for a sample 7-slide capabilities deck in MS Classic style.

---

## Customizing Generated Decks

All generated HTML files use CSS custom properties. Edit the `:root` block at the top of the `<style>` tag to change:

```css
:root {
    --ms-pistachio: #74A686;   /* primary accent color */
    --ms-lavender-1: #F6F4FA;  /* background */
    /* ... */
}
```

Font swap: update the `<link>` tags in `<head>` for Cardo and Proxima Nova.

---

## Original Credit

Built on top of [frontend-slides](https://github.com/zarazhangrui/frontend-slides) by [@zarazhangrui](https://github.com/zarazhangrui) — an excellent Claude Code skill for zero-dependency HTML presentations. Middle Seat brand adaptation by the Middle Seat team.
