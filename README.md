# pottery-glaze

Generative simulations of pottery glazes — layered noise and metaball systems standing in for molten base, drips, crystal blooms, and mineral speckle. Two standalone HTML files, no build step, no dependencies.

## Files

**`index.html`** — full interactive generator with a live tuning panel.
- WebGL layer: cosine multi-stop gradient, simplex UV warp (flow), vertical streak advection (drips), ridged-noise relief, color-shift grain.
- Canvas2D overlay: crystalline blooms + speckle bursts, deterministic from the same seed.
- All state lives in two data blocks, `C` (scalar controls) and `PAL` (palette). The control panel is generated from a `SCHEMA` array, not hand-built — add a row to `SCHEMA` and a slider appears.
- "Copy state" serializes the current seed/controls/palette to clipboard; "Paste state" bakes a copied state back in as defaults.

**`glaze-tiles.html`** — one glaze recipe fired sixteen ways, like a studio test board. Same layered generator as the playground; a seeded recipe varies palette, bloom density, speckle pooling, contrast, and dark-firing per tile.

## Running it

No server needed — open either file directly in a browser, or serve the folder locally:

```
python3 -m http.server
```

## Controls

**Playground:** `R` reroll seed · `H` toggle panel · `S` save PNG
**Tiles:** click a tile to re-fire it · `R` re-fire the whole board · `S` save board as PNG

## Palette system

Presets live in the `PRESETS` object in `glaze-playground.html` — each is 9 hex values (a 4-stop flow gradient, crystal fill/rim/glow, speckle main/accent) named after real glaze combinations (e.g. `Eggplant + Sea Spray`, `Celadon Bloom`). Adding a preset is just adding an entry to that object.

## Notes carried from the creative coding library

- `mulberry32` seeded RNG — same seed always produces the same tile (Art Blocks pattern).
- Noise perturbs gradient *position*, not RGB — flecks and grain pull toward neighboring glaze colors instead of gray.
- Metaball blooms use a compact kernel with merged contours; drift is a pure function of time + a hashed per-bloom identity, not per-frame randomness.
- Base motion crossfades three static noise fields at 120°-phased weights (summing to 1) for continuous morphing without per-frame noise recomputation.

