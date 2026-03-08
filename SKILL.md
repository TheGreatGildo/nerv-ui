---
name: nerv-ui
description: NERV Operations Console aesthetic for web interfaces — Evangelion-inspired instrumentation UI with black void backgrounds, sparse high-contrast color coding (orange headers, green nominal data, cyan wireframes, red alerts), dense monospaced data readouts, measurement overlay grids, hex scanner patterns, CRT/VHS texture layers, escalating alert states, bilingual JP/EN institutional labeling, thermal imaging degradation, and the clinical authority of a fictional military-scientific organization's total visual identity system. Spans CRT terminals → tactical displays → physical infrastructure.
---

# NERV UI — Operations Console Aesthetic

Build web interfaces that feel like they were designed by engineers inside NERV — not by graphic designers outside it. This is the total visual identity of a fictional military-scientific organization: CRT terminals, tactical maps, neural sync diagnostics, contamination scanners, and access control panels. Every screen assumes the operator already knows what they're looking at.

**The screen is off until data demands it. Black void is the default state.**

---

## Design Tokens

### Color Palette

```css
:root {
  /* Void — the screen is OFF until data appears */
  --void:             #000000;  /* True black — primary background */
  --void-warm:        #0A0A08;  /* Slightly warm black for panels */
  --void-panel:       #111110;  /* Raised surface (rare) */

  /* NERV Orange — headers, labels, institutional text */
  --nerv-orange:      #FF9830;  /* Primary labeling color — high contrast */
  --nerv-orange-dim:  #D08028;  /* Secondary labels */
  --nerv-orange-hot:  #FFCC50;  /* Highlighted/active labels */

  /* Nominal Green — data readouts, "system OK" — full phosphor intensity */
  --data-green:       #50FF50;  /* Primary data color */
  --data-green-dim:   #30BB30;  /* Secondary data */
  --data-green-faint: rgba(80, 255, 80, 0.1);  /* Table row hover */

  /* Cyan — wireframes, maps, spatial data */
  --wire-cyan:        #20F0FF;  /* Wireframe lines */
  --wire-cyan-dim:    #10A8B8;  /* Construction lines */
  --wire-cyan-glow:   rgba(32, 240, 255, 0.15);  /* Subtle glow */

  /* Alert Red — warnings, refusals, critical states */
  --alert-red:        #FF4840;  /* Primary alert */
  --alert-red-dim:    #CC3028;  /* Background alert panels */
  --alert-red-hot:    #FF6858;  /* Flashing/urgent */
  --alert-red-fill:   rgba(255, 72, 64, 0.18);  /* Alert zone background */

  /* Thermal spectrum — for degradation/heatmap states */
  --thermal-yellow:   #FFE820;
  --thermal-magenta:  #F030C0;
  --thermal-blue:     #3060F0;
  --thermal-purple:   #A030E0;

  /* Neutral — secondary text, rules */
  --steel:            #E0E0D8;  /* Primary readable text */
  --steel-dim:        #9A9A90;  /* Muted annotations */
  --steel-faint:      rgba(224, 224, 216, 0.08);  /* Grid lines */
}
```

**Rules:**
- Background is ALWAYS true black or near-black. Never gray, never navy.
- Orange is for **labels and headers only** — never data values.
- Green is for **data and nominal status** — numbers, readouts, "OK" states.
- Cyan is for **spatial/structural** — wireframes, maps, grids, containment lines.
- Red is for **alerts and refusals only** — never decoration.
- Colors exist in isolation. They don't blend, gradient, or harmonize. Each serves one function.
- **Phosphor-bright, not muted.** These are CRT colors — they glow. If it looks dim against black, bump it up. Readability always beats subtlety.
- When a system is under stress, colors **invade each other's space** — red bleeds into green zones, thermal spectrum replaces orderly color coding.

### Typography

```css
:root {
  --font-sys:    'IBM Plex Mono', 'Courier New', monospace;   /* Primary — everything */
  --font-label:  'Bebas Neue', 'Arial Narrow', sans-serif;    /* Institutional labels */
  --font-stamp:  'Bebas Neue', 'Impact', sans-serif;          /* REFUSED / WARNING stamps */
  --font-kanji:  'Noto Sans JP', sans-serif;                  /* Japanese institutional text */
}
```

```html
<link href="https://fonts.googleapis.com/css2?family=IBM+Plex+Mono:wght@400;500;700&family=Bebas+Neue&family=Noto+Sans+JP:wght@400;700&display=swap" rel="stylesheet">
```

**Monospace dominates everything.** Bebas Neue is ONLY for large institutional labels and status stamps. The UI assumes fixed-width alignment is load-bearing — columns must align perfectly.

### Type Scale

| Role | Font | Size | Weight | Spacing | Transform | Color |
|------|------|------|--------|---------|-----------|-------|
| Stamp | stamp | `clamp(3rem, 8vw, 6rem)` | 400 | `0.08em` | `uppercase` | `--alert-red` |
| Institutional label | label | `clamp(1.2rem, 3vw, 2rem)` | 400 | `0.15em` | `uppercase` | `--nerv-orange` |
| Section header | sys | `0.875rem` | 700 | `0.12em` | `uppercase` | `--nerv-orange` |
| Data value | sys | `0.8125rem` | 500 | `0em` | `none` | `--data-green` |
| Data label | sys | `0.6875rem` | 400 | `0.08em` | `uppercase` | `--nerv-orange-dim` |
| Annotation | sys | `0.625rem` | 400 | `0.05em` | `uppercase` | `--steel-dim` |
| Scale marker | sys | `0.5rem` | 400 | `0.1em` | `none` | `--nerv-orange` |
| Kanji badge | kanji | `0.75rem` | 700 | `0.05em` | `none` | `--steel` |

---

## CRT / Scanline Texture Layer

Every NERV screen has a CRT texture. This is non-negotiable.

```css
/* Scanline overlay — apply to body or main container */
.crt-overlay {
  position: relative;
}
.crt-overlay::after {
  content: '';
  position: fixed;
  inset: 0;
  background: repeating-linear-gradient(
    0deg,
    transparent,
    transparent 2px,
    rgba(0, 0, 0, 0.06) 2px,
    rgba(0, 0, 0, 0.06) 4px
  );
  pointer-events: none;
  z-index: 9999;
}

/* CRT vignette — darkened edges */
.crt-vignette::before {
  content: '';
  position: fixed;
  inset: 0;
  background: radial-gradient(
    ellipse at center,
    transparent 60%,
    rgba(0, 0, 0, 0.35) 100%
  );
  pointer-events: none;
  z-index: 9998;
}

/* Phosphor flicker (subtle) */
@keyframes phosphor {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.97; }
}
.crt-flicker {
  animation: phosphor 0.08s infinite;
}
```

```css
/* Reduced motion: kill flicker, keep scanlines */
@media (prefers-reduced-motion: reduce) {
  .crt-flicker { animation: none !important; }
}
```

---

## Grid Systems

### Hex Grid Scanner

For detection/anomaly displays — a regular hexagonal mesh where individual cells light up.

```css
.hex-grid {
  --hex-size: 40px;
  --hex-gap: 4px;
  --hex-color: var(--wire-cyan-dim);
  --hex-alert: var(--alert-red);
  display: grid;
  grid-template-columns: repeat(auto-fill, var(--hex-size));
  gap: var(--hex-gap);
  position: relative;
}

.hex-cell {
  width: var(--hex-size);
  height: calc(var(--hex-size) * 1.1547);
  clip-path: polygon(50% 0%, 100% 25%, 100% 75%, 50% 100%, 0% 75%, 0% 25%);
  background: var(--void-panel);
  border: 1px solid var(--hex-color);
  transition: background 0.3s, box-shadow 0.3s;
}
.hex-cell.alert {
  background: var(--alert-red-fill);
  border-color: var(--alert-red);
  box-shadow: 0 0 12px rgba(232, 48, 32, 0.4);
}
.hex-cell.nominal {
  background: rgba(48, 200, 48, 0.06);
  border-color: var(--data-green-dim);
}
```

### Measurement Grid Overlay

Crosshair markers, scale bars, and registration marks.

```css
.measurement-overlay {
  position: relative;
}
.measurement-overlay::before {
  content: '';
  position: absolute;
  inset: 0;
  background-image:
    /* Major grid */
    linear-gradient(var(--steel-faint) 1px, transparent 1px),
    linear-gradient(90deg, var(--steel-faint) 1px, transparent 1px);
  background-size: 80px 80px;
  pointer-events: none;
  z-index: 1;
}

/* Crosshair registration mark */
.crosshair {
  position: absolute;
  width: 24px;
  height: 24px;
}
.crosshair::before,
.crosshair::after {
  content: '';
  position: absolute;
  background: var(--data-green);
}
.crosshair::before {
  width: 1px; height: 100%;
  left: 50%; top: 0;
}
.crosshair::after {
  width: 100%; height: 1px;
  top: 50%; left: 0;
}

/* Scale bar */
.scale-bar {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  font-family: var(--font-sys);
  font-size: 0.5rem;
  letter-spacing: 0.1em;
  color: var(--nerv-orange);
}
.scale-bar::before {
  content: '';
  width: 60px;
  height: 1px;
  background: var(--nerv-orange);
}
```

### Axis Scale (Y-axis ruler)

```css
.y-axis {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  font-family: var(--font-sys);
  font-size: 0.5rem;
  color: var(--nerv-orange);
  padding: 0 0.5rem;
  border-right: 1px solid var(--nerv-orange-dim);
}
.y-axis .tick {
  position: relative;
}
.y-axis .tick::after {
  content: '';
  position: absolute;
  right: -0.5rem;
  top: 50%;
  width: 6px;
  height: 1px;
  background: var(--nerv-orange);
}
```

---

## Components

### Data Table (Column-Aligned Terminal)

```css
.nerv-table {
  width: 100%;
  border-collapse: collapse;
  font-family: var(--font-sys);
  font-size: 0.8125rem;
}
.nerv-table th {
  font-size: 0.625rem;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  text-align: left;
  padding: 0.5rem 0.8rem;
  color: var(--nerv-orange);
  border-bottom: 1px solid var(--nerv-orange-dim);
  font-weight: 400;
}
.nerv-table td {
  padding: 0.4rem 0.8rem;
  color: var(--data-green);
  border-bottom: 1px solid rgba(48, 200, 48, 0.06);
  font-variant-numeric: tabular-nums;
}
.nerv-table td.label {
  color: var(--nerv-orange-dim);
  text-transform: uppercase;
  letter-spacing: 0.06em;
  font-size: 0.6875rem;
}
.nerv-table tr:hover td {
  background: var(--data-green-faint);
}
/* Alert row */
.nerv-table tr.alert td {
  color: var(--alert-red);
  background: var(--alert-red-fill);
}
```

### Command/Response Block

The from/to/command structured header with a giant status stamp.

```css
.command-block {
  border-top: 2px solid var(--nerv-orange);
  padding: 1.5rem;
  position: relative;
  background: var(--void);
}
.command-block .cmd-header {
  font-family: var(--font-sys);
  font-size: 0.6875rem;
  letter-spacing: 0.06em;
  text-transform: uppercase;
  color: var(--nerv-orange);
  line-height: 1.8;
}
.command-block .cmd-header span {
  color: var(--steel);
}
.command-block .cmd-stamp {
  position: absolute;
  top: 50%; right: 2rem;
  transform: translateY(-50%) rotate(-5deg);
  font-family: var(--font-stamp);
  font-size: clamp(3rem, 8vw, 5rem);
  letter-spacing: 0.08em;
  padding: 0.2em 0.5em;
  border: 4px solid;
}
.cmd-stamp.refused {
  color: var(--alert-red);
  border-color: var(--alert-red);
}
.cmd-stamp.approved {
  color: var(--data-green);
  border-color: var(--data-green);
}
.cmd-stamp.pending {
  color: var(--nerv-orange);
  border-color: var(--nerv-orange);
}
```

```html
<div class="command-block">
  <div class="cmd-header">
    from: <span>BALTHASAR DIRECT CONNECTION NO. 102</span><br>
    to: <span>EVA-01 ENTRY PLUG SYSTEM LINK 2</span><br>
    cmd: <span>ENTRY PLUG EJECT</span>
  </div>
  <div class="cmd-stamp refused">REFUSED</div>
</div>
```

### Alert Banner (Full-Width Warning)

```css
.alert-banner {
  background: var(--alert-red);
  color: var(--void);
  padding: 0.6rem 1.5rem;
  font-family: var(--font-stamp);
  font-size: clamp(1.5rem, 4vw, 2.5rem);
  letter-spacing: 0.15em;
  text-transform: uppercase;
  text-align: center;
  position: relative;
  overflow: hidden;
}
/* Chevron arrows flanking */
.alert-banner::before { content: '◀ ◀ ◀  '; }
.alert-banner::after  { content: '  ▶ ▶ ▶'; }

/* Pulsing variant */
@keyframes alert-pulse {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.7; }
}
.alert-banner.pulse {
  animation: alert-pulse 1s ease-in-out infinite;
}
```

### Hazard Stripe Pattern

Environmental/signage stripes — red diagonal on black.

```css
.hazard-stripe {
  background: repeating-linear-gradient(
    -45deg,
    var(--alert-red) 0px,
    var(--alert-red) 20px,
    var(--void) 20px,
    var(--void) 40px
  );
  padding: 1rem 2rem;
}
.hazard-stripe .label {
  font-family: var(--font-stamp);
  font-size: clamp(1.5rem, 4vw, 3rem);
  letter-spacing: 0.15em;
  text-transform: uppercase;
  color: #FFFFFF;
  text-shadow: 2px 2px 0 var(--void);
}
```

### Institutional Label Block

The NERV subject identification panel — SAMPLE 0001 / PATTERN: BLUE TYPE

```css
.id-block {
  border: 1px solid var(--nerv-orange-dim);
  padding: 0.8rem 1rem;
  font-family: var(--font-sys);
  background: var(--void);
  max-width: 280px;
}
.id-block .id-title {
  font-family: var(--font-label);
  font-size: 1.2rem;
  letter-spacing: 0.12em;
  color: var(--nerv-orange);
  text-transform: uppercase;
  margin-bottom: 0.4rem;
}
.id-block .id-row {
  display: flex;
  justify-content: space-between;
  font-size: 0.625rem;
  letter-spacing: 0.06em;
  text-transform: uppercase;
  color: var(--steel-dim);
  padding: 0.15rem 0;
}
.id-block .id-row .value {
  color: var(--data-green);
}
.id-block .id-caution {
  margin-top: 0.4rem;
  padding: 0.2rem 0.5rem;
  background: var(--alert-red);
  color: var(--void);
  font-size: 0.5625rem;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  display: inline-block;
}
```

```html
<div class="id-block">
  <div class="id-title">REI AYANAMI</div>
  <div class="id-row"><span>AGE</span><span class="value">14</span></div>
  <div class="id-row"><span>SAMPLE</span><span class="value">0001</span></div>
  <div class="id-row"><span>PATTERN</span><span class="value">BLOOD TYPE BLUE</span></div>
  <span class="id-caution">⚠ CAUTION</span>
</div>
```

### Neuron Inventory (Data Cascade)

Dense multi-column data listing like the EVA sync diagnostic.

```css
.neuron-cascade {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 0 2rem;
  font-family: var(--font-sys);
  font-size: 0.6875rem;
  line-height: 1.6;
}
.neuron-cascade .col-header {
  font-size: 0.5625rem;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: var(--nerv-orange);
  border-bottom: 1px solid var(--nerv-orange-dim);
  padding-bottom: 0.3rem;
  margin-bottom: 0.3rem;
}
.neuron-cascade .entry {
  color: var(--data-green);
  padding: 0.1rem 0;
}
.neuron-cascade .entry.alert {
  color: var(--alert-red);
}
.neuron-cascade .entry .tag {
  color: var(--nerv-orange-dim);
  margin-right: 0.5rem;
  font-size: 0.5625rem;
}
```

### System Boot / Diagnostics Block

DOS-style system check readout.

```css
.boot-sequence {
  font-family: var(--font-sys);
  font-size: 0.75rem;
  color: var(--alert-red);
  line-height: 1.7;
  padding: 1.5rem;
  background: var(--void);
  overflow-x: auto;
}
.boot-sequence .check-ok {
  color: var(--data-green);
}
.boot-sequence .check-ok::after {
  content: ' Check OK';
  color: var(--data-green);
}
.boot-sequence .addr {
  color: var(--steel-dim);
  margin-right: 1em;
  font-variant-numeric: tabular-nums;
}
.boot-sequence .separator {
  border: none;
  border-top: 1px dashed var(--steel-dim);
  margin: 0.5rem 0;
}
```

### Access Panel (Lock/Permission)

```css
.access-panel {
  background: linear-gradient(180deg, #1A1A18 0%, #0E0E0C 100%);
  border: 2px solid #333330;
  padding: 2rem;
  font-family: var(--font-sys);
  text-align: center;
  max-width: 360px;
  position: relative;
}
.access-panel .panel-title {
  font-size: 0.625rem;
  letter-spacing: 0.15em;
  text-transform: uppercase;
  color: var(--nerv-orange);
  margin-bottom: 1rem;
}
.access-panel .lock-status {
  font-family: var(--font-stamp);
  font-size: 2.5rem;
  letter-spacing: 0.1em;
  margin: 0.5rem 0;
}
.access-panel .lock-status.locked { color: var(--alert-red); }
.access-panel .lock-status.open { color: var(--data-green); }
.access-panel .waiting {
  font-size: 0.6875rem;
  color: var(--nerv-orange-dim);
  letter-spacing: 0.06em;
  text-transform: uppercase;
}
/* LED indicator */
.led {
  display: inline-block;
  width: 8px; height: 8px;
  border-radius: 50% !important;
  margin-right: 0.5rem;
}
.led.red { background: var(--alert-red); box-shadow: 0 0 6px var(--alert-red); }
.led.green { background: var(--data-green); box-shadow: 0 0 6px var(--data-green); }
.led.orange { background: var(--nerv-orange); box-shadow: 0 0 6px var(--nerv-orange); }
```

### Tactical Map Overlay

```css
.tac-map {
  position: relative;
  background: var(--void);
  border: 1px solid var(--wire-cyan-dim);
  overflow: hidden;
}
.tac-map .grid-lines {
  position: absolute;
  inset: 0;
  background-image:
    linear-gradient(var(--wire-cyan-dim) 1px, transparent 1px),
    linear-gradient(90deg, var(--wire-cyan-dim) 1px, transparent 1px);
  background-size: 60px 60px;
  opacity: 0.15;
}
.tac-map .entity {
  position: absolute;
  font-family: var(--font-sys);
  font-size: 0.5625rem;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  padding: 0.2rem 0.5rem;
  border: 1px solid;
}
.entity.friendly {
  color: var(--wire-cyan);
  border-color: var(--wire-cyan);
}
.entity.hostile {
  color: var(--alert-red);
  border-color: var(--alert-red);
  background: var(--alert-red-fill);
}
.entity.unknown {
  color: var(--nerv-orange);
  border-color: var(--nerv-orange);
}
/* Position marker (X) */
.tac-marker {
  position: absolute;
  color: var(--wire-cyan);
  font-family: var(--font-sys);
  font-size: 1rem;
  font-weight: 700;
  line-height: 1;
}
/* Kanji badge */
.kanji-badge {
  position: absolute;
  font-family: var(--font-kanji);
  font-size: 0.75rem;
  font-weight: 700;
  color: var(--steel);
  background: var(--void);
  padding: 0.2rem 0.5rem;
  border: 1px solid var(--steel-dim);
}
```

### Thermal Degradation State

When systems are stressed, visuals break down into stepped color blocks.

```css
/* Stepped gradient — the system losing control */
.thermal-degrade {
  background: linear-gradient(
    135deg,
    var(--thermal-yellow) 0%,
    var(--thermal-yellow) 14%,
    var(--nerv-orange) 14%,
    var(--nerv-orange) 28%,
    var(--alert-red) 28%,
    var(--alert-red) 42%,
    var(--thermal-magenta) 42%,
    var(--thermal-magenta) 57%,
    var(--thermal-purple) 57%,
    var(--thermal-purple) 71%,
    var(--thermal-blue) 71%,
    var(--thermal-blue) 85%,
    #101030 85%,
    #101030 100%
  );
  image-rendering: pixelated;
}

/* Thermal grid — numbered crosshairs over heat */
.thermal-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(3, 1fr);
  position: relative;
}
.thermal-grid .cell {
  border: 1px solid rgba(200, 200, 192, 0.15);
  display: flex;
  align-items: flex-start;
  justify-content: flex-end;
  padding: 0.3rem;
  font-family: var(--font-sys);
  font-size: 0.5rem;
  color: var(--steel-dim);
  min-height: 80px;
}
```

### Waveform Display

Oscilloscope-style animated sine waves.

```css
.waveform-container {
  position: relative;
  height: 200px;
  background: var(--void);
  overflow: hidden;
  border: 1px solid var(--wire-cyan-dim);
}
.waveform-container canvas {
  width: 100%;
  height: 100%;
}
/* Timecode readout overlay */
.timecode {
  position: absolute;
  bottom: 0.5rem;
  right: 0.5rem;
  font-family: var(--font-sys);
  font-size: 0.5625rem;
  color: var(--nerv-orange);
  letter-spacing: 0.1em;
}
```

### Navigation (Minimal)

```css
.nerv-nav {
  display: flex;
  align-items: center;
  padding: 0.5rem 1rem;
  background: var(--void);
  border-bottom: 1px solid var(--nerv-orange-dim);
  gap: 1.5rem;
}
.nerv-nav .org-mark {
  font-family: var(--font-label);
  font-size: 0.875rem;
  letter-spacing: 0.2em;
  color: var(--nerv-orange);
  text-transform: uppercase;
}
.nerv-nav a {
  font-family: var(--font-sys);
  font-size: 0.5625rem;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--steel-dim);
  text-decoration: none;
}
.nerv-nav a:hover,
.nerv-nav a.active {
  color: var(--nerv-orange);
}
```

### Buttons

```css
.nerv-btn {
  display: inline-flex;
  align-items: center;
  gap: 0.4em;
  padding: 0.5em 1.2em;
  font-family: var(--font-sys);
  font-size: 0.6875rem;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  border: 1px solid var(--nerv-orange-dim);
  background: transparent;
  color: var(--nerv-orange);
  cursor: pointer;
  transition: all 0.15s;
}
.nerv-btn:hover {
  background: rgba(232, 122, 32, 0.1);
  border-color: var(--nerv-orange);
}
.nerv-btn.primary {
  background: var(--nerv-orange);
  color: var(--void);
  border-color: var(--nerv-orange);
}
.nerv-btn.primary:hover {
  background: var(--nerv-orange-hot);
}
.nerv-btn.danger {
  border-color: var(--alert-red);
  color: var(--alert-red);
}
.nerv-btn.danger:hover {
  background: var(--alert-red-fill);
}
```

---

## UI States — Escalating Intensity

NERV UIs have **5 visual states** that escalate in intensity. The UI literally degrades as severity increases.

| State | Background | Primary Color | Texture | Animation |
|-------|-----------|---------------|---------|-----------|
| **Nominal** | `--void` | `--data-green` | Clean scanlines | None |
| **Active** | `--void` | `--nerv-orange` | Scanlines | Subtle data flicker |
| **Caution** | `--void` | `--nerv-orange-hot` | Scanlines + hex glow | LED blink |
| **Alert** | `--alert-red-fill` | `--alert-red` | Heavy scanlines | Pulse animation |
| **Critical** | `--thermal-degrade` | White on thermal | Pixelated, stepped | Full screen flash |

```css
/* State classes — apply to containers */
.state-nominal  { /* default — no extra styles */ }
.state-active   { border-color: var(--nerv-orange) !important; }
.state-caution  { border-color: var(--nerv-orange-hot) !important; box-shadow: 0 0 20px rgba(232, 122, 32, 0.1); }
.state-alert    { background: var(--alert-red-fill) !important; border-color: var(--alert-red) !important; }
.state-critical { animation: critical-flash 0.5s ease-in-out infinite; }

@keyframes critical-flash {
  0%, 100% { background: var(--void); }
  50% { background: var(--alert-red-fill); }
}
```

---

## Layout

### Full-Screen Console

```css
body.nerv-console {
  margin: 0;
  padding: 0;
  background: var(--void);
  color: var(--data-green);
  font-family: var(--font-sys);
  font-size: 0.8125rem;
  overflow-x: hidden;
  cursor: default;
}
```

### Panel Grid

```css
.console-grid {
  display: grid;
  grid-template-columns: repeat(12, 1fr);
  gap: 2px;
  padding: 2px;
  height: 100vh;
}
.console-grid > .panel {
  background: var(--void-warm);
  border: 1px solid var(--steel-faint);
  padding: 1rem;
  overflow: hidden;
  position: relative;
}
```

### Ticker Tape (Running Text)

```css
.ticker {
  overflow: hidden;
  white-space: nowrap;
  background: var(--void);
  border-top: 1px solid var(--nerv-orange-dim);
  border-bottom: 1px solid var(--nerv-orange-dim);
  padding: 0.3rem 0;
  font-family: var(--font-sys);
  font-size: 0.5625rem;
  letter-spacing: 0.06em;
  text-transform: uppercase;
  color: var(--nerv-orange);
}
.ticker .scroll {
  display: inline-block;
  animation: ticker-scroll 30s linear infinite;
}
@keyframes ticker-scroll {
  from { transform: translateX(100vw); }
  to   { transform: translateX(-100%); }
}
```

---

## Image Generation Prompts

When using Nano Banana or other image generators to create assets for this style, use these prompt templates:

### Background Textures
```
NERV operations console CRT screen, [SUBJECT], black void background, 
scanline texture overlay, [COLOR] monochrome data visualization, 
no text, no UI chrome, dark atmospheric, film grain, 1990s anime 
production design quality, Hideaki Anno visual style
```

### Subject Overlays (wireframe/thermal)
```
[SUBJECT] rendered as [wireframe mesh / thermal heatmap / contour map], 
[cyan/orange/green] lines on pure black background, crosshair 
registration markers, scale bar annotations, scientific instrumentation 
aesthetic, CRT monitor capture, retro digital imaging, high contrast
```

### Alert/Status Graphics
```
Warning screen, industrial hazard aesthetic, bold [red/orange] 
typography on black, diagonal hazard stripes, institutional signage, 
NERV Evangelion UI style, minimal, stark contrast, Japanese and 
English bilingual text
```

### Hex Grid / Scanner
```
Hexagonal grid scanner display, [cyan/green] hex mesh on black, 
[red/orange] anomaly glow bleeding through [N] cells, scientific 
monitoring interface, CRT texture, dark atmospheric, clean geometric
```

---

## Bilingual Labeling

NERV interfaces mix Japanese and English systematically:

```css
/* Japanese institutional badge */
.jp-badge {
  font-family: var(--font-kanji);
  font-size: 0.6875rem;
  font-weight: 700;
  color: var(--steel);
  writing-mode: horizontal-tb;
  padding: 0.2rem 0.5rem;
  border: 1px solid var(--steel-dim);
  background: var(--void);
}

/* Bilingual label pair */
.bilingual {
  display: flex;
  flex-direction: column;
  gap: 0.15rem;
}
.bilingual .en {
  font-family: var(--font-sys);
  font-size: 0.625rem;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--nerv-orange);
}
.bilingual .jp {
  font-family: var(--font-kanji);
  font-size: 0.75rem;
  color: var(--steel-dim);
}
```

**Pattern:** English for technical/operational labels. Japanese for organizational/institutional labels. Never decorative — every Japanese string should be a real term.

Common terms:
- 作戦行動 (sakusen kōdō) — Operational Action
- 警告 (keikoku) — Warning
- 認証システム (ninshō shisutemu) — Authentication System
- 汚染区域 (osen kuiki) — Contaminated Area
- 緊急 (kinkyū) — Emergency

---

## Anti-Patterns

| ❌ Don't | ✅ Do |
|----------|-------|
| Gray or navy backgrounds | True black (`#000`) always |
| Gradients between colors | Hard color boundaries, stepped blocks |
| Rounded corners anywhere | Sharp corners only (except LEDs/dots) |
| Colorful decorative elements | Color = function (label/data/alert/spatial) |
| Smooth animations | Stepped/flickering/CRT-style motion |
| Friendly/casual copy | Cold, institutional, technical |
| Multiple fonts per context | Mono everywhere, Bebas for stamps only |
| Light/white UI variants | This aesthetic has no light mode |
| Decorative Japanese text | Real terms only, institutional labeling |
| Clean, pixel-perfect rendering | Scanlines, grain, phosphor glow |

---

## Accessibility

**Contrast is king.** These are CRT phosphor colors — they should GLOW against the void. If text looks dim or washed out, you've gone too dark. Err on the side of too bright.

- Orange `#FF9830` on black `#000`: 8.2:1 ✅ (AAA)
- Green `#50FF50` on black: 13.8:1 ✅ (AAA)
- Cyan `#20F0FF` on black: 13.1:1 ✅ (AAA)
- Red `#FF4840` on black: 5.9:1 ✅ (AA) — pair with text label for small text
- Steel `#E0E0D8` on black: 16.2:1 ✅ (AAA)
- Steel-dim `#9A9A90` on black: 7.1:1 ✅ (AAA) — safe for annotations

**Scanline overlay must be subtle (≤6% opacity).** Heavier scanlines kill readability on small text. The CRT effect is atmosphere, not a readability obstacle.

**Vignette must not darken text areas.** Keep the clear zone at ≥60% of the viewport.

```css
@media (prefers-reduced-motion: reduce) {
  .crt-flicker,
  .alert-banner.pulse,
  .state-critical { animation: none !important; }
  .ticker .scroll { animation-duration: 60s; }
}
@media (prefers-contrast: more) {
  :root {
    --nerv-orange: #FFB030;
    --alert-red: #FF6050;
    --data-green: #40FF40;
  }
}
```

---

## Composition Rules

1. **The void is the default.** Elements emerge from blackness, not sit on a surface.
2. **Color = function.** If you can't name why something is orange vs green, it's wrong.
3. **Density = authority.** More data on screen = more credible. Don't spread things out.
4. **Alignment is sacred.** Monospaced columns must align. Pixel-level precision.
5. **Escalation is visual.** As severity increases, the UI literally breaks down — color bleeds, pixelation increases, animation intensifies.
6. **Everything is labeled.** Scale bars, grid coordinates, sample numbers, pattern types. If it's on screen, it has an identifier.
7. **Bilingual is institutional, not decorative.** Japanese text means the organization operates in Japanese. It's not wallpaper.
8. **The CRT texture is always present.** Scanlines, vignette, subtle phosphor flicker. The interface exists on a physical monitor inside the fiction.
