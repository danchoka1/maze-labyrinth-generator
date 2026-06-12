# Labyrinth Site — Progress Report
*Updated 2026-06-13 (evening: drafting room + maze background)*

## What the site is
Single-page immersive experience at `hero.html`: a torch-lit 3D corridor (Three.js) leads to a
bronze labyrinth cylinder; touching it descends into **The Forge**, a maze-builder app
(GSAP-animated) that unwraps a cylinder, carves a perfect maze, and exports PNG/DXF.
The original utilitarian tool remains at `index.html` ("the engineer's bench", linked in the footer).

## How to run it yourself (no Claude needed)
The site is fully static — any web server pointed at `F:\Puzzle` works. It cannot be opened
via `file://` because the Three.js module imports and texture fetches need HTTP.

```
cd F:\Puzzle
python -m http.server 3333
```
Then open in a browser:
- http://localhost:3333/hero.html — **the main site** (corridor → Forge)
- http://localhost:3333/index.html — the original engineer's bench (generator + DXF viewer)
- http://localhost:3333/hero_v1_backup.html — backup of the corridor-only hero (pre-Forge)
- http://localhost:3333/experience.html — archived WASD-walkable maze experiment

Alternatives if Python isn't on PATH: `npx serve -l 3333 .` or VS Code's Live Server extension.
Stop the server with Ctrl+C in that terminal.

## Done and verified
- **Hero corridor** (user-approved, do not rework): preloader with real load %, ENTER gate,
  scroll-driven dolly, torch flicker, particle fog, lazy EXR sky + lazy 10MB GLB, proximity
  raycast on a proxy cylinder, "Touch the artifact" interaction, descent transition.
- **Chrome stripped**: only the LABYRINTH wordmark (top-left). No nav links, no scrollbar,
  no progress bar, no scroll hint.
- **Hard gate**: story is `display:none` until the artifact is touched → page physically ends
  at the corridor's last step; scrolling cannot pass it.
- **Refresh fix**: `history.scrollRestoration = 'manual'` + scrollTo(0,0) in head — refreshing
  anywhere returns to the corridor mouth; no more stranded-at-artifact state.
- **The Forge (builder section)**:
  - Inputs: cylinder diameter / height / channel width (mm) as editorial underline fields
  - Live readout: unwrapped surface (π·d × h) and grid size
  - Temperament: i. Gentle (DFS) / ii. Winding (hybrid) / iii. Merciless (Prim's)
  - **Animated carve**: the maze draws itself by replaying the algorithm's own steps (1.7s)
  - **Ariadne's thread**: solution path reveals as a glowing ember line, gate to gate
  - Exports: PNG (ink/bone plan) and DXF (mm units, MAZE layer — same format as the original tool)
  - A11y: labels, radiogroup, aria-live status, canvas aria-label, reduced-motion = instant
- **Exports click-tested (2026-06-13)**: intercepted the real download blobs in-browser.
  - DXF `labyrinth-23x14.dxf`: valid AC1015 header, `$INSUNITS 4` (mm), MAZE layer,
    1,296 LINE entities, EOF terminator (105 KB)
  - **Round-trip**: the generated DXF was loaded into index.html's viewer — parsed all
    1,296 entities, rendered the maze at correct mm scale with both gates visible
  - PNG `labyrinth-23x14.png`: real decodable image/png, 1000×640
- **Mobile pass on the Forge (2026-06-13)**: 390×844 — fields stack, readout wraps, carve +
  thread + caption all work; no horizontal overflow (all sections measure 390px).
- **Bug fixed (2026-06-13)**: `#plan-actions` used the `hidden` attribute but its
  `display:flex` CSS rule overrode it, so thread/PNG/DXF buttons were visible before any
  maze was carved. Fixed with `#plan-actions[hidden] { display: none; }`.
- **Console clean**: no JS errors; only harmless `THREE.DataUtils.toHalfFloat(): Value out
  of range` warnings from overbright EXR pixels.
- **03 · The Drafting Room (2026-06-13)** — the engineer's bench features absorbed into the
  site; the footer link to index.html removed (the file itself remains on disk, unlinked):
  - DXF viewer ported from index.html, recolored to the design system (ink-2 canvas, bone
    lines, ember measure overlay), DPR-sharp
  - Drop a file or click to browse (LINE / LWPOLYLINE / CIRCLE / ARC); drag-over highlight
  - Fit / Closer / Further toolbar, wheel zoom at cursor, drag pan, touch pinch + pan
  - Measure tool with endpoint > midpoint > nearest-on-line snapping (16px radius),
    live coords readout in mm, distance result in Cormorant numerals
  - "Open in the drafting room ↓" in plan-actions loads the carved maze instantly
    (shared buildForgeDXF() refactored out of the export handler)
  - Eject returns to the dropzone
  - Verified: carve → open in room (1,296 lines), zoom/pan, measure snapped to an exact
    endpoint (28.000 mm between grid endpoints), eject, drag-drop of a synthetic
    square+circle DXF (68 lines), mobile 390px (no overflow), DXF download still valid
- **Interactive maze background (2026-06-13)**: full-viewport canvas behind the story
  sections draws a faint DFS maze (bone hairlines at 7.5%); walls within 170px of the
  cursor heat up ember and cool down (decay 0.945/frame); gentle scroll drift (6%);
  radial mask keeps the reading column clear; IntersectionObserver gates the rAF loop;
  static-only under reduced motion. Verified ember pixels light along pointer sweeps.
- **Bug fixed (2026-06-13)**: after the descent, scrolling back to the corridor/story
  boundary re-showed "Touch the artifact". The previously-unused `finished` flag now
  guards both the proximity branch and triggerDescent().

## Remaining / nice-to-have
1. **Multi-agent review panel**: workflow script saved at
   `C:\Users\danie\.claude\projects\F--Puzzle\4dd769d4-...\workflows\scripts\labyrinth-site-review-*.js`
   — failed on session limits before; can re-run for design/motion/a11y findings.
2. **Deploy**: push to GitHub → Vercel (vercel.json already has cleanUrls; site lives at /hero).
   Note: many generated screenshots/scratch files in the repo root are uncommitted — tidy or
   .gitignore them before pushing.
3. Optional polish: sound design (torch crackle, carve scratch), seed input for reproducible
   mazes, "send plan to workshop" mailto with grid params.

## Key files
- `hero.html` — the site (everything inline). Backup of pre-forge version: `hero_v1_backup.html`
- `index.html` — original generator + DXF viewer (untouched, linked as footer tool)
- `DESIGN.md` / `PRODUCT.md` — design system + brief (impeccable-skill format)
- `libs/gsap.min.js`, `libs/ScrollTrigger.min.js` — vendored
- `images/opt/` — optimized assets (wall_2k, floor_2k, cylinder.glb)
- Local dev: `.claude/launch.json` → python http.server :3333
