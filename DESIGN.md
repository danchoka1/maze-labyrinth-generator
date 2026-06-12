# DESIGN.md — Labyrinth

## Theme

Dark, forced by the scene: the visitor is underground in a torch-lit stone
corridor at night. Light mode is impossible in this world.

## Color (OKLCH-minded, committed strategy)

The surface is near-black warm stone; one amber accent carries the identity.
Neutrals are tinted toward the flame hue, never pure black/white.

| Token        | Value                  | Use |
|--------------|------------------------|-----|
| --ink        | `#0a0705`              | page background (warm near-black) |
| --ink-2      | `#120d08`              | raised chamber background |
| --bone       | `#e8ddcc`              | primary text (warm off-white) |
| --bone-dim   | `#9a8d78`              | secondary text |
| --bone-faint | `#5e5445`              | tertiary text, rules |
| --ember      | `#d9a05b`              | the single accent: links, glows, markers |
| --ember-hot  | `#f2c285`              | accent hover / highlights |
| --line       | `rgba(232,221,204,.14)`| hairline rules |

Accent budget: ember appears on ≤10% of any viewport.

## Typography

- Display: **Cormorant Garamond** 300/400 + italic — long elegant serif,
  editorial not medieval. Tracking normal at large sizes.
- Labels/UI: **Space Grotesk** 400/500 — uppercase micro-labels,
  letter-spacing 0.25–0.4em, 10–12px.
- Body: Space Grotesk 400, 15–17px, line-height 1.8, max 64ch.
- Scale contrast ≥1.25 between steps; hero display uses clamp(64px → 180px).

## Layout

- Full-bleed chambers, generous vertical rhythm (160–240px between sections).
- Asymmetric editorial grids; no identical card rows; content alternates
  left/right down the page like switchbacks in a descent.
- Hairline rules (1px --line) structure content instead of boxes/cards.
- Numbered sections: 01 / 02 / 03 in micro-label style.

## Motion (GSAP + Three.js)

- Register: brand site → Jakub primary (polish), Jhey secondary (delight),
  Emil for nav/controls.
- Entrances: y 40→0 + opacity, 0.9–1.2s, `expo.out`, stagger 60–90ms.
- Scrubbed: camera dolly tied to scroll (lerp-smoothed), section parallax ≤8%.
- Micro: 150–300ms, ease-out. Exits ~65% of enter duration.
- No bounce, no elastic, no animating layout properties (transform/opacity only).
- `prefers-reduced-motion`: kill parallax + scrub smoothing, reveals become
  near-instant fades, camera jumps without easing, fog drift stops.

## Components

- Custom cursor: 6px ember dot + 28px ring, ring lags by lerp; ring expands
  on interactive targets. Desktop fine-pointer only.
- Buttons: 1px bone-faint border, pill, uppercase micro-label; hover floods
  border to ember with 250ms ease; subtle magnetic pull (≤6px) on desktop.
- Grain: fixed SVG-noise overlay at 4–5% opacity, `pointer-events:none`.
- Progress: 1px ember hairline at top of viewport, scaleX = scroll progress.

## Bans (inherited + project)

No side-stripe accents, no gradient text, no glassmorphism, no emoji icons,
no em dashes in copy, no pure #000/#fff, no card grids.
