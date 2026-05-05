# Frontend Design Briefing

You are about to write or edit frontend code for this project. Before touching any HTML/CSS, internalize everything below.

---

## Brand Tokens

### Colors (use these exact hex values — never default Tailwind palette)
| Token | Hex | Usage |
|---|---|---|
| Primary | `#383A30` | Nav, footer, dark surfaces |
| Secondary | `#46505F` | Body text, muted elements |
| Accent | `#C79B78` | CTAs, labels, highlights |
| Navy | `#0A1738` | Dark CTA sections, reviews bg |
| Cream | `#F8F7F4` | Page background |
| Text | `#0C0C0E` | Default body text |
| Olive | `#6A7161` | Subtle surface variant |
| Gold | `#997558` | Secondary accent |

### Typography
- **Headings**: `Libre Baskerville`, serif — weight 400, tracking `-0.02em` on large sizes
- **Body / UI**: `Inter`, sans-serif — weight 300, line-height 1.3, letter-spacing `0.1em`
- **Labels**: Inter, 11px, uppercase, weight 400–500, letter-spacing `0.2–0.3em`, color `#C79B78`
- **Buttons**: Inter, 11px, uppercase, weight 500, letter-spacing `0.2em`
- Always load both from Google Fonts: `Libre Baskerville:ital,wght@0,400;0,700;1,400` + `Inter:wght@300;400;500;600`

### Spacing system
- Section padding: `80px` vertical, `60px` horizontal
- Card gaps: `24–32px`
- Label → heading gap: `14–18px`
- Heading → body gap: `20–24px`
- Body → CTA gap: `36–44px`

---

## Design Rules (hard requirements)

### Shadows
Never `shadow-md`. Use layered, color-tinted shadows:
```css
box-shadow: 0 2px 8px rgba(56,58,48,0.08), 0 8px 32px rgba(56,58,48,0.12);
```

### Gradients
Always layer multiple radial gradients. Add grain/texture via SVG noise filter:
```html
<svg style="position:absolute;inset:0;width:100%;height:100%;opacity:0.05;pointer-events:none;">
  <filter id="grain"><feTurbulence type="fractalNoise" baseFrequency="0.65" numOctaves="3" stitchTiles="stitch"/>
  <feColorMatrix type="saturate" values="0"/></filter>
  <rect width="100%" height="100%" filter="url(#grain)"/>
</svg>
```

### Animations
- Only animate `transform` and `opacity` — never `transition-all`
- Use spring-style easing: `cubic-bezier(0.25, 0.46, 0.45, 0.94)`
- Image hover: `transform: scale(1.04)` with `transition: transform 0.4s cubic-bezier(...)`

### Interactive states
Every clickable element needs all three:
- `hover` — color/opacity shift
- `focus-visible` — visible outline, never remove
- `active` — scale down slightly (`transform: scale(0.98)`)

### Images
- Always wrap in `position: relative; overflow: hidden`
- Always add gradient overlay: `linear-gradient(to top, rgba(10,23,56,0.55) 0%, transparent 55%)`
- Use `https://placehold.co/WIDTHxHEIGHT/BGCOLOR/TEXTCOLOR` for placeholders — never leave raw `<img>` without dimensions
- Never use a placeholder `?text=` with an empty value — it causes dimension text to render

### Depth / layering
Surfaces must have a clear z-plane system:
- Base: `#F8F7F4` (cream)
- Elevated: `rgba(255,255,255,0.6)` with backdrop-filter or subtle border
- Floating: white with layered shadow

---

## Pre-flight Checklist (run before writing any code)

1. **Check `brand_assets/`** — use any real logos, colors, or images found there. Never use placeholders where real assets exist.
2. **Check for a reference image** — if one exists, you MUST match it exactly. No improvements, no additions.
3. **Confirm server is running** — `node serve.mjs` on port 3000. Start in background if not running.
4. **Plan sections top-to-bottom** — list every section before writing HTML.

---

## Screenshot & Comparison Workflow

```bash
# Start server (if not already running)
Start-Process node -ArgumentList "serve.mjs" -WindowStyle Hidden

# Take screenshot
node screenshot.mjs http://localhost:3000 label

# Screenshots auto-save to: ./temporary screenshots/screenshot-N-label.png
# Read the PNG with the Read tool to view and compare
```

### Comparison rounds
- **Round 1**: Full-page screenshot → compare overall structure, section order, color blocks
- **Round 2**: Crop individual sections → compare spacing (px-level), font size/weight, colors (exact hex), border-radius, shadows
- **Stop only when**: no visible differences remain, OR user says to stop
- **Be specific**: "heading gap is 24px but reference shows ~16px" — not "looks close"

---

## Common Mistakes to Avoid

- Using `transition-all` — forbidden
- Using Tailwind's default `blue-600`, `indigo-500` — forbidden
- Same font for headings and body — forbidden
- Flat `shadow-md` — forbidden
- Screenshotting a `file:///` URL — always use `http://localhost:3000`
- Stopping after one screenshot pass — always do at least two rounds
- Adding sections not in the reference — match only what exists

---

## Output Format

- Single `index.html`, all styles inline, unless told otherwise
- Tailwind CDN: `<script src="https://cdn.tailwindcss.com"></script>`
- Mobile-first, responsive
- No comments in the HTML unless the WHY is genuinely non-obvious
