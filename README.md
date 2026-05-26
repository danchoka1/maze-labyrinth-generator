# Maze Labyrinth Generator

A browser-based tool for designing and exporting cylindrical maze labyrinths.

## Features

- **Step 1** — Enter cylinder diameter + height → auto-calculates the unrolled rectangle (circumference × height)
- **Step 2** — Set cell size (mm) + difficulty → grid dimensions auto-fill; generate a perfect maze (exactly 1 solution, no loops)
- **Step 3** — Preview with optional solution overlay; export as **PNG** or **DXF** (1:1 mm scale)
- **Step 4** — Built-in DXF viewer with pan, zoom, and **snap-to-geometry measurement tool**

## Maze algorithms

| Difficulty | Algorithm | Style |
|---|---|---|
| Normal | Recursive DFS | Long winding corridors |
| Medium | Hybrid DFS (random stack pick) | Balanced branching |
| Hard | Prim's algorithm | Bushy, many short dead ends |

## DXF Viewer — Snap types

| Symbol | Snap | Triggered when |
|---|---|---|
| 🟡 Gold square | **Endpoint** | Cursor near a line endpoint / corner |
| 🔵 Cyan triangle | **Midpoint** | Cursor near the centre of a line |
| 🔵 Cyan circle+cross | **Nearest** | Cursor near any point on a line |

## Running locally

```bash
npx serve . --listen 3333
# then open http://localhost:3333
```

Or double-click **run.bat**.

## Deployment

Deployed as a static site — no build step needed.  
Works on Vercel, Netlify, GitHub Pages, or any static host.
