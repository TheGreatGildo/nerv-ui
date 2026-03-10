# NERV UI

A Claude Code / OpenClaw skill and component library for building web interfaces with the **NERV Operations Console** aesthetic — the visual identity of a fictional military-scientific organization from Neon Genesis Evangelion.

**The screen is off until data demands it. Black void is the default state.**

## What's In The Box

### Skill File
`SKILL.md` — the complete v2 design system specification. Drop it into `~/.claude/skills/nerv-ui/` and Claude will build interfaces in this style. Covers design tokens, typography, CRT effects, all component patterns, emergency mode, and composition rules.

### Drop-in Stylesheet
`nerv-ui.css` — 808 lines, zero dependencies (except fonts). Contains all design tokens, CRT effects, and component classes. Link it and go.

### Component Demos
Each is self-contained HTML — open in browser, see it working, extract what you need.

```
components/
├── event-log.html    — Blockchain event log terminal (color-coded scrolling feed)
├── vault-card.html   — EVA Unit-style vault status cards
├── magi-oracle.html  — Three-source oracle price consensus display
├── metrics-grid.html — Dense key protocol metrics grid
├── data-table.html   — Strategy + Transmuter performance tables
└── crt-effects.css   — CRT overlay effects (standalone, use anywhere)
```

### Full Demo
`demo.html` — complete working dashboard with Three.js MAGI visualization, event log, vault cards, oracle display, metrics, data tables, and emergency mode toggle.

### v1 Archive
`v1-old/SKILL.md` — the original v1 spec (Cormorant Garamond, hex grids, command blocks). Preserved for reference.

## The Style

### Color System

CRT phosphor colors — they glow against true black. Each has exactly one job:

| Color | Hex | Role |
|-------|-----|------|
| **NERV Orange** | `#FF9830` | Headers, labels, classification |
| **Data Green** | `#50FF50` | Data readouts, nominal status |
| **Wire Cyan** | `#20F0FF` | Wireframes, spatial data |
| **Alert Red** | `#FF3030` | Emergencies only |
| **Steel** | `#D8D8D0` | Secondary text, annotations |

Colors exist in isolation — no blending, no gradients. When systems are under stress, colors invade each other's space.

### Typography — NO HELVETICA

| Face | Role |
|------|------|
| **Noto Serif Display** (weight 900) | English titles — mechanically compressed via `scaleX(0.82)` |
| **Shippori Mincho B1** | Japanese institutional text |
| **JetBrains Mono** | All data readouts, terminal text, tables |
| **Saira Extra Condensed** | WARNING stamps, emergency banners |

The mechanical compression (`scaleX(0.78–0.85)`) on heavy serif titles is the signature EVA look. No Helvetica anywhere.

### CRT Effects

Every screen has: scanlines, edge vignette, phosphor flicker, and a moving scan line. These are atmosphere — they never compromise readability. Scanline opacity capped at 6%.

### Emergency Mode

Toggle `data-mode="emergency"` on `<html>`. All green turns red, cyan turns red, scan line speeds up, emergency overlay appears, MAGI goes into conflict state.

### 3D MAGI Visualization (Three.js)

The hero element. A data-driven scene with:
- Cyan wireframe mesh — undulating yield surface with numbered vertex labels
- Orange organic curves — 30 flow lines rising from the mesh, independently swaying
- Glowing tip dots — pulsing orange spheres at curve endpoints
- Flanking bar columns — orange metric bars with wireframe outlines
- Green background particles — scattered depth points
- Slow camera orbit

## v2 vs v1

| v1 | v2 |
|---|---|
| Cormorant Garamond (max 700) | **Noto Serif Display** (weight 900) — heavier |
| Decorative 3D wireframes | **Data-driven MAGI visualization** |
| Generic icosphere globe | **Animated terrain mesh** with organic curves |
| No event stream | **Blockchain event log** terminal |
| Moderate density | **Maximum density** — every panel shows data |
| Hex grid scanner | Cut — replaced with strategy performance table |
| Command/ID blocks | Cut — replaced with data-driven metrics |

## Usage

### As a Claude Code Skill

```
cp SKILL.md ~/.claude/skills/nerv-ui/SKILL.md
```

Claude will reference the skill when building interfaces in this style.

### As a CSS Library

```html
<link href="https://fonts.googleapis.com/css2?family=Noto+Serif+Display:wght@700;800;900&family=JetBrains+Mono:wght@400;500;700&family=Saira+Extra+Condensed:wght@400;600;700;800&family=Shippori+Mincho+B1:wght@500;700;800&display=swap" rel="stylesheet">
<link rel="stylesheet" href="nerv-ui.css">
```

### CRT Effects Only

Just want the CRT look? Use `components/crt-effects.css` standalone and add `<div class="scan-line-overlay"></div>` to your body.

## Anti-Patterns

| Don't | Do |
|-------|-----|
| Gray/navy backgrounds | True black (#000) |
| Helvetica | Use the specified font stack |
| Decorative 3D wireframes | Data-driven visualizations |
| Smooth gradients | Hard boundaries, stepped blocks |
| Rounded corners | Sharp corners (except LEDs) |
| Light mode | No light variant exists |
| Decorative Japanese | Real institutional terms only |
| Low density layouts | Density = authority |
| Static numbers | Live-updating from shared data model |

## Accessibility

All primary colors pass WCAG AA or AAA against black:

- Orange `#FF9830` — 8.2:1 (AAA)
- Green `#50FF50` — 13.8:1 (AAA)
- Cyan `#20F0FF` — 13.1:1 (AAA)
- Red `#FF3030` — 5.9:1 (AA)

Respects `prefers-reduced-motion`.

## License

MIT — free to use, modify, and distribute.
