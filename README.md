# Labyrinth · A Puzzle Cast in Bronze

**[Live site →](https://project-zy44r.vercel.app/hero)**

An immersive single-page experience that leads you through a torch-lit 3D corridor to a bronze labyrinth cylinder — then descends into a maze-builder that produces a real, cuttable artefact.

---

## Experience

**The Corridor** — A scroll-driven dolly through a stone passage lit by torch flicker and particle fog. A bronze cylinder waits at the end. Walk toward it. Touch it.

**The Forge** — Enter cylinder dimensions (diameter × height × channel width in mm) and choose a temperament:
- *i. Gentle* — depth-first, long winding passages
- *ii. Winding* — hybrid algorithm, unpredictable turns
- *iii. Merciless* — Prim's, dense and disorienting

Watch the maze carve itself in real time. Reveal Ariadne's thread. Export the unwrapped plan as **PNG** or **DXF** (millimetre units, ready for a laser cutter or CNC).

**The Drafting Room** — Drop any DXF file to inspect it: pan, zoom, pinch. A measure tool with endpoint / midpoint / nearest-on-line snap reads distances in mm. The Forge plan opens here directly — no download needed.

**The Background** — A faint DFS maze covers the story sections. Walls within 170 px of the cursor heat up to ember and cool as you move away.

---

## Stack

| Layer | Technology |
|---|---|
| 3D scene | Three.js r158 (local ES module) |
| Animations | GSAP 3.12 + ScrollTrigger |
| Fonts | Cormorant Garamond · Space Grotesk |
| Hosting | Vercel (static, `cleanUrls`) |

No build step. No framework. One HTML file.

---

## Run locally

The site uses ES module imports — it must be served over HTTP, not `file://`.

```bash
cd maze-labyrinth-generator
python -m http.server 3333
```

Open **http://localhost:3333/hero** in a browser.

Alternatives:
```bash
npx serve -l 3333 .
# or VS Code → Live Server extension
# or double-click run.bat (Windows)
```

---

## Repository layout

```
hero.html          # the site (everything inline)
vercel.json        # cleanUrls + root redirect to /hero
run.bat            # one-click local server (Windows)
libs/              # GSAP 3.12 + ScrollTrigger (vendored)
vendor/            # Three.js r158 + loaders (vendored)
images/opt/        # optimised runtime assets
  cylinder.glb       # bronze labyrinth model (10 MB)
  wall_2k.jpg        # corridor wall texture
  floor_2k.jpg       # corridor floor texture
images/
  puzzle.mp4         # story section video
  moonless_golf_4k.exr  # HDR night sky
docs/              # design system, product brief, build report
```

---

## Design tokens

| Token | Value | Role |
|---|---|---|
| `--ink` | `#0a0705` | Background |
| `--bone` | `#e8ddcc` | Primary text |
| `--ember` | `#d9a05b` | Accent / interactive |
| `--ember-hot` | `#f2c285` | Highlight |
