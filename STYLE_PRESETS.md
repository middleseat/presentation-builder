# Middle Seat Style Presets

Four curated visual presets for generating Middle Seat presentations. Every preset uses the Middle Seat brand typography and color system — they differ in background tone, layout energy, and animation character.

Read [MS_BRAND.md](MS_BRAND.md) for complete CSS components, font loading, and logo embedding instructions.

---

## Preset 1: MS Classic

**Mood:** Professional, editorial, trustworthy  
**Best for:** Internal briefings, general-purpose presentations, campaign status updates

### Signature Look

- Background: lavender_1 `#F6F4FA` — the primary MS background
- Headings: Cardo Regular, slate_2 `#363140`
- Body: Proxima Nova, slate_1 `#4D4759`
- Labels/data: Antarctican Mono, lavender_6 `#7E778C`
- Accent: pistachio `#74A686` — progress bar, stat numbers, card borders, accent bars
- Header bar on every interior slide with MS horizontal logo (dark version)
- Cover: `linear-gradient(135deg, #F6F4FA 0%, #EAE6F2 100%)`
- Pistachio accent bar (3px) on cover between logo and title
- Animations: Clean fade-up with staggered reveals (`translateY(20px)` → `translateY(0)`, 0.5s ease-out)
- Nav dots: pistachio fill, lavender_3 for inactive

### CSS Variables (add to `:root`)

```css
/* MS Classic additions */
--preset-bg: var(--ms-lavender-1);
--preset-surface: var(--ms-lavender-2);
--preset-border: var(--ms-lavender-3);
--preset-heading: var(--ms-slate-2);
--preset-body: var(--ms-slate-1);
--preset-muted: var(--ms-lavender-6);
--preset-accent: var(--ms-pistachio);
--preset-accent-2: var(--ms-lime);
```

### Key Layout Signatures

- Cover: Logo centered top → pistachio accent bar → large Cardo title → Antarctican Mono subtitle
- Interior header: Left-aligned logo, 1px lavender_3 bottom border
- Slide content top-padded to clear header (~60px)
- Section intro slides: Left-aligned `h2` + pistachio 40px wide `border-left` vertical accent
- Bullet points: `::before` pseudo-element using pistachio circle (6px diameter)
- Closing: Peppermint gradient background, white Cardo heading, white logo

### Recommended for These Slide Types

Title, content, feature grid, stat callout, table, quote, closing

---

## Preset 2: MS Bold

**Mood:** High-impact, confident, campaign-energy  
**Best for:** Campaign pitches, fundraising asks, launch announcements, urgent calls to action

### Signature Look

- Background: slate_2/3 dark `linear-gradient(135deg, #363140 0%, #201D26 100%)`
- Headings: Cardo, white `#FFFFFF`
- Body: Proxima Nova, lavender_1 `#F6F4FA` (slightly softened from pure white)
- Labels: Antarctican Mono, `rgba(246, 244, 250, 0.6)` (muted white)
- Primary accent: pistachio `#74A686` — glows, borders, CTAs
- Secondary accent: cantaloupe `#F2A26D` — for contrast against pistachio
- MS logo: WHITE version (replace `#16151A` fills with `#FFFFFF`)
- Cover: Full-bleed dark bg, pistachio left border `8px` on title, white Cardo heading
- Animations: Bold punch — `scale(0.96)` → `scale(1)` + fade-in, 0.4s ease-out. More immediate than Classic.
- Stat numbers: Extra large Cardo in pistachio, drop-shadow: `0 0 30px rgba(116,166,134,0.3)`

### CSS Variables (add to `:root`)

```css
/* MS Bold additions */
--preset-bg: #201D26;
--preset-surface: rgba(255, 255, 255, 0.06);
--preset-border: rgba(255, 255, 255, 0.1);
--preset-heading: #FFFFFF;
--preset-body: #F6F4FA;
--preset-muted: rgba(246, 244, 250, 0.55);
--preset-accent: var(--ms-pistachio);
--preset-accent-2: var(--ms-cantaloupe);
```

### Key Layout Signatures

- Cover: Full-dark background, logo centered (white), large left-aligned Cardo title with 8px pistachio left-border, Antarctican Mono subtitle in muted-white
- Interior slides: No header bar — logo appears as small subtle element bottom-right (`position: absolute; bottom: 20px; right: 20px; opacity: 0.5`)
- Feature cards: Glassmorphism cards — `background: rgba(255,255,255,0.05); backdrop-filter: blur(10px); border: 1px solid rgba(255,255,255,0.1); border-radius: 10px`
- Stats: Centered pistachio numbers with cantaloupe accent bars above
- Bullet points: `::before` pistachio `—` dash (en dash, not bullet)
- Closing: Pistachio-to-lime Peppermint gradient, white text, centered

### Recommended for These Slide Types

Title, content, stat callout, feature grid, closing. Avoid dense table slides in MS Bold.

---

## Preset 3: MS Data

**Mood:** Analytical, evidence-forward, rigorous  
**Best for:** Polling reports, fundraising dashboards, digital performance reviews, research presentations

### Signature Look

- Background: lavender_1 `#F6F4FA` (same as Classic)
- Headings: Cardo, slate_2 `#363140`
- Body: Proxima Nova, slate_1 `#4D4759`
- Labels/data: Antarctican Mono — prominently used, more than any other preset
- Pistachio `#74A686` for leading metrics, key numbers, chart color #1
- Cantaloupe `#F2A26D` for chart color #2
- Layout: Dense information hierarchy — multiple stats visible per slide
- Animations: Subtle — numbers count up on entry (`data-target` counter JS), bars slide in
- Tables: Always fully styled with alternating rows, leader highlighting

### CSS Variables (add to `:root`)

```css
/* MS Data additions */
--preset-bg: var(--ms-lavender-1);
--preset-surface: var(--ms-lavender-2);
--preset-border: var(--ms-lavender-3);
--preset-heading: var(--ms-slate-2);
--preset-body: var(--ms-slate-1);
--preset-muted: var(--ms-lavender-6);
--preset-accent: var(--ms-pistachio);
--preset-accent-2: var(--ms-cantaloupe);
```

### Key Layout Signatures

- Cover: Same gradient as Classic, but subtitle uses Antarctican Mono with "PREPARED BY MIDDLE SEAT" footer
- Interior header: Same as Classic, but adds slide-level `data-label` in top-right (Antarctican Mono, lavender_6)
- Stat callout slides: 2-3 stats in a grid. Each `stat-box` has pistachio `border-top: 3px`. Numbers animate counting up on `.visible`
- Chart placeholder areas: `lavender_2` fills with dashed `lavender_3` border and centered Antarctican Mono label: "CHART AREA"
- Table slides: Full-width tables with all standard MS table styles. First column bold. Leader row highlighted in lime.
- Color dot legend: Use `.dot` class with correct color order (see MS_BRAND.md)
- Footer on every slide: 1px `lavender_3` top border, Antarctican Mono `CONFIDENTIAL · MIDDLE SEAT · [MONTH YEAR]`

### Counter Animation JS

Add to SlidePresentation class for stat slides:

```javascript
animateCounters(slide) {
    slide.querySelectorAll('[data-target]').forEach(el => {
        const target = parseFloat(el.dataset.target);
        const suffix = el.dataset.suffix || '';
        const prefix = el.dataset.prefix || '';
        const isDecimal = el.dataset.decimal === 'true';
        let current = 0;
        const increment = target / 60;
        const timer = setInterval(() => {
            current = Math.min(current + increment, target);
            el.textContent = prefix + (isDecimal ? current.toFixed(1) : Math.round(current)) + suffix;
            if (current >= target) clearInterval(timer);
        }, 16);
    });
}
```

Usage in HTML:
```html
<span class="number" data-target="47" data-suffix="%">0%</span>
```

### Recommended for These Slide Types

Stat callout (primary), table, content with bullets, chart area placeholder. Cover and closing same as MS Classic.

---

## Preset 4: MS Proposal

**Mood:** Warm, professional, relationship-first  
**Best for:** New business pitches, client onboarding, agency overview, partnership proposals

### Signature Look

- Cover: Full-bleed Peppermint gradient (`linear-gradient(135deg, #74A686 0%, #CED998 100%)`), white logo, white Cardo title
- Interior backgrounds: White `#FFFFFF` (warmer than Classic's lavender)
- Section divider slides: lavender_1 background with large Cardo section number (pistachio, very large, semi-transparent)
- Headings: Cardo, slate_2
- Body: Proxima Nova, slate_1
- Pull-quote slides: pistachio `4px` left border, italic Cardo quote, Antarctican Mono attribution
- Timeline/process slides: Horizontal steps with pistachio connectors and numbered circles
- Closing: Grapefruit gradient background (`linear-gradient(135deg, #FFBCAD 0%, #FFADAD 100%)`), slate_2 text (warm, inviting)
- Animations: Slightly slower, more graceful — 0.6s ease-out, elements slide in from `translateY(24px)`

### CSS Variables (add to `:root`)

```css
/* MS Proposal additions */
--preset-bg: #FFFFFF;
--preset-surface: var(--ms-lavender-1);
--preset-border: var(--ms-lavender-3);
--preset-heading: var(--ms-slate-2);
--preset-body: var(--ms-slate-1);
--preset-muted: var(--ms-lavender-6);
--preset-accent: var(--ms-pistachio);
--preset-accent-2: var(--ms-cantaloupe);
```

### Key Layout Signatures

- Cover: White MS logo centered, below it the Cardo title in white, Antarctican Mono subtitle in `rgba(255,255,255,0.75)`, pistachio accent bar
- Interior slides: Thin left-rail accent — 4px pistachio vertical bar, 32px left padding to content
- Section intro slides: Large Cardo numeral (`01`, `02`) in `rgba(116,166,134,0.15)` as background watermark, section title overlaid in slate_2
- Process/timeline: Numbered circles (pistachio bg, white number) connected by pistachio dashed line
- Pull-quote: `border-left: 4px solid var(--ms-pistachio)` on `<blockquote>`, italic Cardo, Antarctican Mono attribution in lavender_6
- Closing: Grapefruit gradient (peach → watermelon), slate_2 headings, pistachio CTA button or link

### Process/Timeline Component

```css
.timeline {
    display: flex;
    align-items: flex-start;
    gap: 0;
    width: 100%;
    max-width: 700px;
}

.timeline-step {
    flex: 1;
    display: flex;
    flex-direction: column;
    align-items: center;
    position: relative;
}

.timeline-step:not(:last-child)::after {
    content: '';
    position: absolute;
    top: 20px;
    left: 50%;
    right: calc(-50% + 0px);
    height: 2px;
    background: linear-gradient(to right, var(--ms-pistachio), var(--ms-lime));
    z-index: 0;
}

.step-circle {
    width: 40px;
    height: 40px;
    border-radius: 50%;
    background: var(--ms-pistachio);
    color: white;
    font-family: 'AntarcticanMono', monospace;
    font-size: 0.75rem;
    display: flex;
    align-items: center;
    justify-content: center;
    position: relative;
    z-index: 1;
    margin-bottom: 0.75em;
}

.step-label {
    font-family: 'Cardo', serif;
    font-size: clamp(0.9rem, 1.5vw, 1.1rem);
    color: var(--ms-slate-2);
    text-align: center;
}

.step-desc {
    font-family: 'proxima-nova', sans-serif;
    font-size: clamp(0.6rem, 0.9vw, 0.75rem);
    color: var(--ms-slate-1);
    text-align: center;
    margin-top: 0.25em;
    line-height: 1.4;
}
```

### Recommended for These Slide Types

Cover (Peppermint), section dividers, content/bullets, pull-quote, process/timeline, feature grid, closing (Grapefruit)
