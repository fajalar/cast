# Spell Meditation — Style Guide

The colours and typefaces behind the spell, as they live in `index.html` and `privacy.html`. A rendered version is at `styleguide.html`.

Dark-only by design (`<meta name="color-scheme" content="dark">`).

## Colour

Defined as CSS custom properties on `:root`.

| Token | Value | Role |
|---|---|---|
| `--void-deep` | `#05060F` | Deepest ground — page background, `theme-color`, button ink |
| `--void` | `#0B0E1A` | Night — background-gradient core, banner base |
| `--ember` | `#C8954A` | Warm start — the sigil's low-charge glow |
| `--arcane` | `#B9A8FF` | The accent — headings, buttons, links, charged sigil |
| `--bright` | `#F4ECD8` | Parchment light — primary text and the cast flash |
| `--dim` | `rgba(244,236,216,0.42)` | `--bright` at 42% — secondary text, prompts, breath cue |
| `--faint` | `rgba(244,236,216,0.16)` | `--bright` at 16% — wordmark, footer, hairline rules |

**The charge.** The palette is a state, not a flat set. As the sigil fills, its light lerps `--ember → --arcane → --bright` (mirrored in JS as `EMBER [200,149,74] → ARCANE [185,168,255] → BRIGHT [244,236,216]`). Ember is idle, arcane is charging, bright is the cast.

**Derived surfaces.** The support box and tip banner reuse `--arcane` at low alpha: background `rgba(185,168,255,0.05)`, borders `rgba(185,168,255,0.22)`–`0.28`. Body copy is `--bright` at 82% (`rgba(244,236,216,0.82)`).

## Type

Two families, two jobs.

- **Cormorant Garamond** — display & body, the voice of the spell. Google Fonts (OFL), loaded via `@import`. Weights in use: **400**, **500**, **600**, plus **400 italic**.
  `font-family: 'Cormorant Garamond', Georgia, 'Times New Roman', serif;`
- **Monospace stack** — utility & controls (capability readout, cast count, buttons). System faces, no download.
  `font-family: ui-monospace, 'SF Mono', Menlo, Consolas, monospace;`

### Scale

Fluid — display sizes are `clamp()` ranges that grow with the viewport (desktop ceiling shown).

| Role | Size | Notes |
|---|---|---|
| Wordmark | `clamp(1rem, 3vw, 1.2rem)` | uppercase, `0.5em` tracking, `--faint` |
| Display H1 | `clamp(2rem, 6vw, 3rem)` | weight 600 |
| Prompt | `clamp(1.5rem, 5vw, 2.4rem)` | weight 500 |
| Section H2 | `clamp(1.3rem, 4vw, 1.7rem)` | weight 600, `--arcane` |
| Lead | `1.3rem` | — |
| Body | `1.25rem` | `--bright` at 82% (footer body is `0.9rem`) |
| Breath cue | `clamp(1.1rem, 3.4vw, 1.5rem)` | lowercase, `0.22em` tracking, `--dim` |
| Buttons | `0.9rem` | mono, `0.03em` tracking |
| Readout / count | `11px` | mono, `--faint` |
