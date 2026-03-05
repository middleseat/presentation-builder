---
name: presentation-builder
description: Create stunning, on-brand HTML presentations in Middle Seat style. Use when building pitch decks, client proposals, campaign briefings, fundraising presentations, or internal reports. Produces zero-dependency single HTML files with animations, viewport-fitted slides, and Middle Seat brand typography and color. Supports creating from scratch or converting PowerPoint files.
---

# Middle Seat Presentation Builder

Create zero-dependency, animation-rich HTML slide decks in Middle Seat brand style — Cardo headings, Proxima Nova body, Antarctican Mono labels, and the full Middle Seat color palette.

## Core Principles

1. **Zero Dependencies** — Single HTML files with inline CSS/JS. No npm, no build tools.
2. **Always On-Brand** — Every deck uses the Middle Seat color system, typography stack, and logo. No exceptions.
3. **Show, Don't Tell** — Generate visual previews so the user picks the right tone before full generation.
4. **Viewport Fitting (NON-NEGOTIABLE)** — Every slide MUST fit exactly within 100vh. No scrolling within slides, ever. Content overflows? Split into multiple slides.

## Viewport Fitting Rules

These invariants apply to EVERY slide in EVERY presentation:

- Every `.slide` must have `height: 100vh; height: 100dvh; overflow: hidden;`
- ALL font sizes and spacing must use `clamp(min, preferred, max)` — never fixed px/rem
- Content containers need `max-height` constraints
- Images: `max-height: min(50vh, 400px)`
- Breakpoints required for heights: 700px, 600px, 500px
- Include `prefers-reduced-motion` support
- Never negate CSS functions directly (`-clamp()`, `-min()`, `-max()` are silently ignored) — use `calc(-1 * clamp(...))` instead

**When generating, read `viewport-base.css` and include its full contents in every presentation.**

### Content Density Limits Per Slide

| Slide Type | Maximum Content |
|------------|-----------------|
| Title slide | 1 heading + 1 subtitle + optional tagline |
| Content slide | 1 heading + 4-6 bullet points OR 1 heading + 2 paragraphs |
| Feature grid | 1 heading + 6 cards maximum (2x3 or 3x2) |
| Stat slide | 2-3 hero stats with Antarctican Mono labels |
| Quote slide | 1 quote (max 3 lines) + attribution |
| Image slide | 1 heading + 1 image (max 60vh height) |
| Table slide | 1 heading + table (max 8 rows visible) |

**Content exceeds limits? Split into multiple slides. Never cram, never scroll.**

---

## Phase 0: Detect Mode

Determine what the user wants:

- **Mode A: New Presentation** — Create from scratch. Go to Phase 1.
- **Mode B: PPT Conversion** — Convert a .pptx file. Go to Phase 4.
- **Mode C: Enhancement** — Improve an existing HTML presentation. Read it, understand it, enhance. Follow Mode C modification rules below.

### Mode C: Modification Rules

When enhancing existing presentations:

1. **Before adding content:** Count existing elements, check against density limits
2. **Adding images:** Must have `max-height: min(50vh, 400px)`. If slide already has max content, split
3. **Adding text:** Max 4-6 bullets per slide. Exceeds limits? Split into continuation slides
4. **After ANY modification, verify:** `.slide` has `overflow: hidden`, new elements use `clamp()`, images have viewport-relative max-height
5. **Proactively reorganize:** If modifications will cause overflow, automatically split content and inform the user

---

## Phase 1: Content Discovery

**Ask ALL questions in a single AskUserQuestion call** so the user fills everything out at once:

**Question 1 — Purpose** (header: "Purpose"):
What type of deck is this? Options: Campaign pitch / Client proposal / Internal briefing / Fundraising ask

**Question 2 — Length** (header: "Length"):
Approximately how many slides? Options: Short 5-10 / Medium 10-20 / Long 20+

**Question 3 — Content** (header: "Content"):
Do you have content ready? Options: All content ready / Rough notes / Topic only

**Question 4 — Inline Editing** (header: "Editing"):
Do you need to edit text directly in the browser after generation? Options:
- "Yes (Recommended)" — Can edit text in-browser, auto-save to localStorage, export file
- "No" — Presentation only, keeps file cleaner

**Remember the user's editing choice — it determines whether edit-related code is included in Phase 3.**

If user has content, ask them to share it.

### Step 1.2: Image Evaluation (if images provided)

If no images → skip to Phase 2.

If user provides images:
1. **Scan** — List all image files (.png, .jpg, .svg, .webp)
2. **View each image** — Use the Read tool (Claude is multimodal)
3. **Evaluate** — USABLE or NOT USABLE (with reason), what concept it represents, dominant colors
4. **Co-design the outline** — Curated images inform slide structure alongside text
5. **Confirm via AskUserQuestion** (header: "Outline"): "Does this slide outline look right?" Options: Looks good / Adjust images / Adjust outline

---

## Phase 2: Style Discovery

**This is the "show, don't tell" phase.** Users discover what they want by seeing it.

### Step 2.0: Style Path

Ask (header: "Style"):
- "Show me options" (recommended) — Generate 3 previews based on mood
- "I know what I want" — Pick from the 4 MS preset list directly

If direct selection: Show the 4 presets from [STYLE_PRESETS.md](STYLE_PRESETS.md) and skip to Phase 3.

### Step 2.1: Mood Selection

Ask (header: "Vibe", multiSelect: true, max 2):
What tone should this deck strike?
- Professional/Credible — Polished, editorial, trustworthy
- Bold/Urgent — High-impact, campaign energy
- Data-Driven — Evidence-forward, analytical
- Warm/Inviting — Relationship-first, new business

### Step 2.2: Generate 3 Style Previews

Based on mood, generate 3 distinct single-slide HTML previews from the MS preset family. Read [STYLE_PRESETS.md](STYLE_PRESETS.md) for full specifications.

| Mood | Suggested Presets |
|------|-------------------|
| Professional/Credible | MS Classic, MS Proposal |
| Bold/Urgent | MS Bold, MS Classic |
| Data-Driven | MS Data, MS Classic |
| Warm/Inviting | MS Proposal, MS Classic |

Save previews to `.claude-design/slide-previews/` (style-a.html, style-b.html, style-c.html). Each should be a self-contained animated title slide, ~60-100 lines. Embed brand fonts and inline the MS logo SVG in each preview.

Open each preview automatically for the user.

### Step 2.3: User Picks

Ask (header: "Style"):
Which style preview do you prefer? Options: Style A: [Name] / Style B: [Name] / Style C: [Name] / Mix elements

If "Mix elements", ask for specifics before proceeding.

---

## Phase 3: Generate Presentation

**Before generating, read these files in order:**
1. [MS_BRAND.md](MS_BRAND.md) — Colors, fonts, logo specs, layout components
2. [STYLE_PRESETS.md](STYLE_PRESETS.md) — Chosen preset's full specification
3. [viewport-base.css](viewport-base.css) — Mandatory responsive CSS
4. [html-template.md](html-template.md) — HTML architecture and JS patterns
5. [animation-patterns.md](animation-patterns.md) — Animation reference for chosen mood

**Brand asset embedding (perform at generation time):**

1. **Antarctican Mono font:** Read `~/middleseat/brand_assets/fonts/AntarcticanMono-Medium.ttf`, Python base64-encode it, embed as `@font-face` in the `<style>` block:
   ```python
   import base64
   with open(os.path.expanduser('~/middleseat/brand_assets/fonts/AntarcticanMono-Medium.ttf'), 'rb') as f:
       b64 = base64.b64encode(f.read()).decode()
   # Paste b64 into the @font-face src URL
   ```

2. **MS Logo (light bg):** Read `~/middleseat/brand_assets/logos/MS-horizontal.svg` and inline the SVG directly in the HTML.

3. **MS Logo (dark bg — MS Bold preset):** Same SVG but replace all `fill="#16151A"` with `fill="#FFFFFF"` before inlining.

4. **Proxima Nova:** `<link rel="stylesheet" href="https://use.typekit.net/wjc3fcc.css">`

5. **Cardo:** `<link href="https://fonts.googleapis.com/css2?family=Cardo:ital,wght@0,400;0,700;1,400&display=swap" rel="stylesheet">`

**Key requirements:**
- Single self-contained HTML file, all CSS/JS inline (except external font CDN links)
- Include the FULL contents of viewport-base.css in the `<style>` block
- Use MS CSS variables (`--ms-pistachio`, `--ms-lavender-1`, etc.) throughout
- Every section gets a clear `/* === SECTION NAME === */` comment block
- Include MS logo on every slide (header bar on interior slides, centered on cover/closing)
- Progress bar uses pistachio (`#74A686`)
- Add detailed comments explaining how to customize each section

---

## Phase 4: PPT Conversion

When converting PowerPoint files:

1. **Extract content** — Run `python scripts/extract-pptx.py <input.pptx> <output_dir>` (install python-pptx if needed: `pip install python-pptx`)
2. **Confirm with user** — Present extracted slide titles, content summaries, and image counts
3. **Style selection** — Proceed to Phase 2 for style discovery
4. **Generate HTML** — Convert to chosen MS style, preserving all text, images (from assets/), slide order, and speaker notes (as HTML comments)

---

## Phase 5: Delivery

1. **Clean up** — Delete `.claude-design/slide-previews/` if it exists
2. **Open** — `open [filename].html` to launch in default browser
3. **Summarize:**
   - File location, preset name, slide count
   - Navigation: Arrow keys, Space, scroll/swipe, click nav dots
   - Customization: edit `:root` CSS variables for colors; swap font CDN links for typography
   - If inline editing enabled: Hover top-left corner or press E to enter edit mode, click any text to edit, Ctrl+S to save

---

## Supporting Files

| File | Purpose | When to Read |
|------|---------|-------------|
| [MS_BRAND.md](MS_BRAND.md) | Complete Middle Seat brand reference | Phase 3 (always) |
| [STYLE_PRESETS.md](STYLE_PRESETS.md) | 4 MS visual presets with full specs | Phase 2 & 3 |
| [viewport-base.css](viewport-base.css) | Mandatory responsive CSS — copy into every presentation | Phase 3 |
| [html-template.md](html-template.md) | HTML architecture, JS features, code quality standards | Phase 3 |
| [animation-patterns.md](animation-patterns.md) | CSS/JS animation snippets and effect-to-feeling guide | Phase 3 |
| [scripts/extract-pptx.py](scripts/extract-pptx.py) | Python script for PPT content extraction | Phase 4 |

---

## Inline Editing Implementation (Opt-In Only)

**If the user chose "No" for inline editing in Phase 1, do NOT generate any edit-related HTML, CSS, or JS.**

**Do NOT use CSS `~` sibling selector for hover-based show/hide.** Use JS-based hover with 400ms delay timeout instead.

HTML:
```html
<div class="edit-hotzone"></div>
<button class="edit-toggle" id="editToggle" title="Edit mode (E)">✏️</button>
```

CSS:
```css
.edit-hotzone {
    position: fixed; top: 0; left: 0;
    width: 80px; height: 80px;
    z-index: 10000;
}
.edit-toggle {
    opacity: 0;
    pointer-events: none;
    transition: opacity 0.3s ease;
    z-index: 10001;
}
.edit-toggle.show, .edit-toggle.active {
    opacity: 1;
    pointer-events: auto;
}
```

JS hover with 400ms grace period:
```javascript
let hideTimeout = null;
const hotzone = document.querySelector('.edit-hotzone');
const editToggle = document.getElementById('editToggle');

hotzone.addEventListener('mouseenter', () => { clearTimeout(hideTimeout); editToggle.classList.add('show'); });
hotzone.addEventListener('mouseleave', () => { hideTimeout = setTimeout(() => { if (!editor.isActive) editToggle.classList.remove('show'); }, 400); });
editToggle.addEventListener('mouseenter', () => clearTimeout(hideTimeout));
editToggle.addEventListener('mouseleave', () => { hideTimeout = setTimeout(() => { if (!editor.isActive) editToggle.classList.remove('show'); }, 400); });
hotzone.addEventListener('click', () => editor.toggleEditMode());
document.addEventListener('keydown', (e) => { if ((e.key === 'e' || e.key === 'E') && !e.target.getAttribute('contenteditable')) editor.toggleEditMode(); });
```
