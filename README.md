# NERV UI

A Claude Code / OpenClaw skill for building web interfaces with the **NERV Operations Console** aesthetic — the visual identity of a fictional military-scientific organization from Neon Genesis Evangelion.

## The Style

Black void. Phosphor glow. The screen is off until data demands it.

Every interface built with this skill looks like it was designed by engineers inside NERV — not by graphic designers outside it. CRT terminals, tactical map overlays, neural sync diagnostics, contamination scanners, and access control panels. Dense, monospaced, institutional.

### Color System

Colors are CRT phosphors — they glow against true black. Each color has exactly one job:

| Color | Hex | Role |
|-------|-----|------|
| **NERV Orange** | `#FF9830` | Headers, labels, institutional text |
| **Data Green** | `#50FF50` | Data readouts, nominal status, "system OK" |
| **Wire Cyan** | `#20F0FF` | Wireframes, maps, spatial/structural data |
| **Alert Red** | `#FF4840` | Warnings, refusals, critical states only |
| **Steel** | `#E0E0D8` | Secondary text, annotations |

Colors never blend, gradient, or harmonize. They exist in isolation. When a system is under stress, colors invade each other's space — red bleeds into green zones, thermal spectrum replaces orderly color coding.

### Typography

Monospace dominates everything. `IBM Plex Mono` is the primary face. `Bebas Neue` is reserved exclusively for large institutional labels and status stamps (REFUSED / APPROVED / WARNING). Fixed-width alignment is load-bearing — columns must align perfectly.

### Texture

Every screen has a CRT texture layer: subtle scanlines (6% opacity), edge vignette, and phosphor flicker. The interface exists on a physical monitor inside the fiction. These effects are atmosphere, not obstacles — they never compromise readability.

### Escalation

The UI has 5 visual states that escalate in intensity. As severity increases, the interface literally degrades:

1. **Nominal** — Clean green data on void, minimal texture
2. **Active** — Orange takes over, data begins flickering
3. **Caution** — Hot orange, hex grid glows, LEDs blink
4. **Alert** — Red fills flood the background, pulse animations
5. **Critical** — Thermal spectrum, pixelated stepped gradients, full screen flash

### Components

The skill includes patterns for: data tables, command/response blocks, alert banners, hazard stripes, institutional ID panels, hex grid scanners, measurement overlays, boot diagnostics, access panels, tactical maps, waveform displays, ticker tapes, and bilingual JP/EN labeling.

### Bilingual Labeling

English for technical/operational labels. Japanese for organizational/institutional labels. Never decorative — every Japanese string is a real term. The organization operates in Japanese.

### Anti-Patterns

- No gray or navy backgrounds. True black always.
- No gradients between colors. Hard boundaries, stepped blocks.
- No rounded corners (except LEDs).
- No friendly or casual copy. Cold, institutional, technical.
- No light mode. This aesthetic has no light variant.
- No decorative Japanese text. Real terms only.

## Usage

### As a Claude Code Skill

Copy the `SKILL.md` file into your Claude Code skills directory:

```
~/.claude/skills/nerv-ui/SKILL.md
```

Claude will reference the skill when building interfaces that match this aesthetic.

### As a Reference

Read `SKILL.md` directly — it contains the complete design system with CSS custom properties, component patterns, layout templates, image generation prompts, and composition rules.

## Accessibility

All primary colors meet WCAG AA or AAA contrast against black:

- Orange `#FF9830` — 8.2:1 (AAA)
- Green `#50FF50` — 13.8:1 (AAA)
- Cyan `#20F0FF` — 13.1:1 (AAA)
- Red `#FF4840` — 5.9:1 (AA)
- Steel `#E0E0D8` — 16.2:1 (AAA)

Respects `prefers-reduced-motion` and `prefers-contrast` media queries.

## License

MIT — free to use, modify, and distribute.
