# Middle Seat Brand Reference

Complete brand specification for generating slide presentations. Read this entire file during Phase 3 before generating any HTML.

---

## Typography

### Font Loading

Always add these to the HTML `<head>` in every generated presentation:

```html
<!-- Proxima Nova (body text) via Adobe Typekit -->
<link rel="stylesheet" href="https://use.typekit.net/wjc3fcc.css">

<!-- Cardo (headings, hero stats, titles) via Google Fonts -->
<link href="https://fonts.googleapis.com/css2?family=Cardo:ital,wght@0,400;0,700;1,400&display=swap" rel="stylesheet">
```

Anarctican Mono is NOT on any CDN — embed it as base64 via `@font-face` (see below).

### Font Stack

| Role | Font | CSS Family | Fallback |
|------|------|-----------|----------|
| Headings, hero stats, cover title | Cardo | `'Cardo'` | `Georgia, serif` |
| Body text, paragraphs, table cells | Proxima Nova | `'proxima-nova'` | `'Helvetica Neue', Helvetica, Arial, sans-serif` |
| Labels, data annotations, small caps, table headers | Antarctican Mono | `'AntarcticanMono'` | `'Courier New', monospace` |

### Embedding Antarctican Mono

At generation time, base64-encode the font file and embed it as a `@font-face` rule:

```python
import base64, os
path = os.path.expanduser('~/middleseat/brand_assets/fonts/AntarcticanMono-Medium.ttf')
with open(path, 'rb') as f:
    b64 = base64.b64encode(f.read()).decode()
# Use b64 in the CSS below
```

Add to the `<style>` block BEFORE all other CSS:

```css
@font-face {
    font-family: 'AntarcticanMono';
    src: url('data:font/truetype;base64,{{B64_FONT_DATA}}') format('truetype');
    font-weight: 500;
    font-style: normal;
    font-display: swap;
}
```

### Type Scale

| Element | Font | Size (clamp) | Weight | Letter Spacing | Line Height |
|---------|------|--------------|--------|---------------|-------------|
| Cover title | Cardo | `clamp(2rem, 5vw, 4rem)` | 400 | -1px | 1.1 |
| Section heading (h2) | Cardo | `clamp(1.5rem, 3.5vw, 2.5rem)` | 400 | — | — |
| Subsection (h3) | Cardo | `clamp(1.1rem, 2.5vw, 1.75rem)` | 400 | — | — |
| Hero stat number | Cardo | `clamp(3rem, 6vw, 5rem)` | 400 | — | 1.0 |
| Hero stat label | Antarctican Mono | `clamp(0.6rem, 1vw, 0.75rem)` | 500 | 2px | — |
| Body text | Proxima Nova | `clamp(0.75rem, 1.2vw, 1rem)` | 400 | — | 1.5 |
| Table header | Antarctican Mono | `clamp(0.55rem, 0.8vw, 0.7rem)` | 500 | 1px | — |
| Subtitle / date / label | Antarctican Mono | `clamp(0.65rem, 1vw, 0.85rem)` | 500 | 4px | — |
| Footer text | Proxima Nova | `clamp(0.6rem, 0.9vw, 0.75rem)` | 400 | — | — |
| Page number | Antarctican Mono | `clamp(0.55rem, 0.8vw, 0.7rem)` | 500 | — | — |

---

## Color Palette

### CSS Custom Properties

Add all of these to `:root` in EVERY generated presentation:

```css
:root {
    /* === LAVENDER SCALE — backgrounds & structure === */
    --ms-lavender-1: #F6F4FA;   /* Page background, chart background */
    --ms-lavender-2: #EAE6F2;   /* Section fills, table headers, alternating rows */
    --ms-lavender-3: #CEC8D9;   /* Borders, gridlines, row dividers */
    --ms-lavender-4: #B3ACBF;   /* Secondary borders, table header bottom border */
    --ms-lavender-6: #7E778C;   /* Muted text, footer text, stat labels */

    /* === SLATE SCALE — text === */
    --ms-slate-1: #4D4759;      /* Primary body text, axis labels */
    --ms-slate-2: #363140;      /* Headings, table header text */
    --ms-slate-3: #201D26;      /* Darkest — used for dark backgrounds */

    /* === GREENS — primary accent === */
    --ms-pistachio: #74A686;    /* Hero stats, leaders, progress bars, CTAs */
    --ms-lime: #CED998;         /* Secondary green, highlight rows, Peppermint gradient */

    /* === WARM ACCENTS === */
    --ms-cantaloupe: #F2A26D;   /* Chart color #1, warning callout borders */
    --ms-orange:     #FFBA8C;   /* Chart color #7 */
    --ms-peach:      #FFBCAD;   /* Background accents, Grapefruit gradient start */
    --ms-watermelon: #FFADAD;   /* Chart color #5, Grapefruit gradient end */

    /* === COOL ACCENTS === */
    --ms-strawberry: #EDC1CC;   /* Chart color #6 */
    --ms-blueberry:  #C98D9C;   /* Chart color #2 */
    --ms-blackberry: #B37D8A;   /* Chart color #3 */

    /* === INTERACTIVE === */
    --ms-cotton-candy: #A0C2FA; /* Link text */
    --ms-blue-moon:    #3A51FE; /* Active/hover link state */

    /* === GRADIENTS === */
    --ms-peppermint:      linear-gradient(to top, #CED998, #74A686);
    --ms-grapefruit:      linear-gradient(to top, #FFBCAD, #FFADAD);
    --ms-cover-gradient:  linear-gradient(135deg, #F6F4FA 0%, #EAE6F2 100%);
    --ms-dark-gradient:   linear-gradient(135deg, #363140 0%, #201D26 100%);
}
```

### Chart Color Assignment Order

When comparing multiple entities (candidates, clients, channels):
- **Leader/winner** → Pistachio `#74A686`
- Non-leaders in order: Cantaloupe → Blueberry → Blackberry → Lime → Watermelon → Strawberry → Orange

---

## Logo

### Source File

```
~/middleseat/brand_assets/logos/MS-horizontal.svg
```

Inline the SVG directly into HTML at generation time (do not use an `<img>` src).

### Light Background Version (default)

Use the SVG as-is — all paths use `fill="#16151A"` (near-black).

### Dark Background Version (MS Bold preset)

Replace all `fill="#16151A"` with `fill="#FFFFFF"` before inlining.

### Sizing

```css
.ms-logo {
    height: clamp(22px, 2.8vh, 34px);
    width: auto;
    display: block;
}
```

---

## Layout Components

### Header Bar (All Interior Slides)

Every non-cover, non-closing slide gets a top header bar:

```html
<header class="slide-header">
    <div class="ms-logo"><!-- INLINE SVG HERE --></div>
</header>
```

```css
.slide-header {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    padding: clamp(8px, 1.2vh, 14px) clamp(1rem, 3vw, 2rem);
    border-bottom: 1px solid var(--ms-lavender-3);
    display: flex;
    align-items: center;
    background: transparent;
    z-index: 10;
}
```

### Cover Page

```css
.title-slide {
    background: var(--ms-cover-gradient);
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    text-align: center;
    padding: var(--slide-padding);
}

.title-slide .logo-container {
    margin-bottom: clamp(1.5rem, 4vh, 3rem);
}

.title-slide h1 {
    font-family: 'Cardo', Georgia, serif;
    font-size: clamp(2rem, 5vw, 4rem);
    color: var(--ms-slate-2);
    letter-spacing: -1px;
    line-height: 1.1;
    margin-bottom: 0.5em;
    max-width: 14ch;
}

.title-slide .subtitle {
    font-family: 'AntarcticanMono', 'Courier New', monospace;
    font-size: clamp(0.6rem, 1vw, 0.8rem);
    color: var(--ms-lavender-6);
    letter-spacing: 4px;
    text-transform: uppercase;
    margin-top: 0.25em;
}

.title-slide .accent-bar {
    width: clamp(40px, 6vw, 80px);
    height: 3px;
    background: var(--ms-pistachio);
    margin: clamp(1rem, 2vh, 1.5rem) auto;
    border-radius: 2px;
}
```

### Stat Callout Boxes

```css
.stat-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(min(100%, 160px), 1fr));
    gap: clamp(0.75rem, 2vw, 1.5rem);
}

.stat-box {
    background: var(--ms-lavender-2);
    border-radius: 10px;
    padding: clamp(1rem, 2vw, 1.75rem);
    text-align: center;
    border-top: 3px solid var(--ms-pistachio);
}

.stat-box .number {
    font-family: 'Cardo', Georgia, serif;
    font-size: clamp(2.5rem, 5vw, 4.5rem);
    color: var(--ms-pistachio);
    line-height: 1;
    display: block;
}

.stat-box .label {
    font-family: 'AntarcticanMono', 'Courier New', monospace;
    font-size: clamp(0.55rem, 0.8vw, 0.7rem);
    color: var(--ms-lavender-6);
    letter-spacing: 2px;
    text-transform: uppercase;
    margin-top: 0.5em;
    display: block;
}
```

### Tables

```css
table { width: 100%; border-collapse: collapse; font-family: 'proxima-nova', sans-serif; }

thead th {
    font-family: 'AntarcticanMono', monospace;
    font-size: clamp(0.55rem, 0.8vw, 0.7rem);
    text-transform: uppercase;
    letter-spacing: 1px;
    color: var(--ms-slate-2);
    background: var(--ms-lavender-2);
    padding: 8px 10px;
    border-bottom: 2px solid var(--ms-lavender-4);
    text-align: left;
}

tbody td {
    font-size: clamp(0.65rem, 1vw, 0.875rem);
    padding: 8px 10px;
    border-bottom: 1px solid var(--ms-lavender-3);
    color: var(--ms-slate-1);
}

tbody tr:nth-child(even) { background: var(--ms-lavender-2); }
tbody tr.leader { background: var(--ms-lime); font-weight: 600; }
```

### Callout Boxes

```css
.callout {
    border-left: 4px solid var(--ms-pistachio);
    background: rgba(116, 166, 134, 0.08);
    padding: 12px 16px;
    border-radius: 0 6px 6px 0;
    font-size: clamp(0.7rem, 1vw, 0.9rem);
    color: var(--ms-slate-1);
    font-family: 'proxima-nova', sans-serif;
    line-height: 1.5;
}

.callout.warning {
    border-color: var(--ms-cantaloupe);
    background: rgba(242, 162, 109, 0.08);
}
```

### Feature/Service Cards

```css
.card-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(min(100%, 200px), 1fr));
    gap: clamp(0.5rem, 1.5vw, 1rem);
}

.card {
    background: var(--ms-lavender-2);
    border-radius: 8px;
    padding: clamp(0.75rem, 1.5vw, 1.25rem);
    border-top: 2px solid var(--ms-pistachio);
}

.card .card-label {
    font-family: 'AntarcticanMono', monospace;
    font-size: clamp(0.55rem, 0.8vw, 0.7rem);
    text-transform: uppercase;
    letter-spacing: 1.5px;
    color: var(--ms-pistachio);
    margin-bottom: 0.5em;
    display: block;
}

.card h3 {
    font-family: 'Cardo', serif;
    font-size: clamp(1rem, 2vw, 1.4rem);
    color: var(--ms-slate-2);
    margin-bottom: 0.4em;
}

.card p {
    font-family: 'proxima-nova', sans-serif;
    font-size: clamp(0.65rem, 1vw, 0.85rem);
    color: var(--ms-slate-1);
    line-height: 1.4;
    margin: 0;
}
```

### Pull Quote Slide

```css
.quote-slide {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: var(--slide-padding);
}

.quote-mark {
    font-family: 'Cardo', serif;
    font-size: clamp(3rem, 8vw, 7rem);
    color: var(--ms-pistachio);
    line-height: 0.5;
    margin-bottom: 0.5em;
    opacity: 0.6;
}

blockquote {
    font-family: 'Cardo', Georgia, serif;
    font-size: clamp(1.2rem, 2.5vw, 2rem);
    color: var(--ms-slate-2);
    line-height: 1.3;
    max-width: 700px;
    text-align: center;
    margin: 0;
    font-style: italic;
}

.quote-attribution {
    font-family: 'AntarcticanMono', monospace;
    font-size: clamp(0.6rem, 0.9vw, 0.75rem);
    color: var(--ms-lavender-6);
    letter-spacing: 2px;
    text-transform: uppercase;
    margin-top: 1.5em;
}
```

### Color Dot Indicators

```css
.dot {
    display: inline-block;
    width: 10px;
    height: 10px;
    border-radius: 50%;
    margin-right: 6px;
    vertical-align: middle;
    flex-shrink: 0;
}
```

### Progress Bar

Always pistachio, always fixed top:

```css
.progress-bar {
    position: fixed;
    top: 0;
    left: 0;
    height: 3px;
    background: var(--ms-pistachio);
    transition: width 0.3s ease;
    z-index: 1000;
    border-radius: 0 2px 2px 0;
}
```

### Closing Slide

Close with the Peppermint gradient (green) or Grapefruit gradient (warm), centered layout, large logo:

```css
.closing-slide {
    background: var(--ms-peppermint);
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    text-align: center;
    padding: var(--slide-padding);
}

.closing-slide h2 {
    font-family: 'Cardo', serif;
    color: #fff;
    font-size: clamp(1.5rem, 3.5vw, 2.5rem);
}

.closing-slide .contact {
    font-family: 'AntarcticanMono', monospace;
    font-size: clamp(0.65rem, 1vw, 0.85rem);
    color: rgba(255,255,255,0.85);
    letter-spacing: 2px;
    margin-top: 1em;
}
```
