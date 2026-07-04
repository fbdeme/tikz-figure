# Visual vocabulary (distilled from NeurIPS-2025 accepted figures, via PaperBanana)

These are the conventions reviewers subconsciously expect. Deviating reads as amateur
even when the content is right.

## Shape semantics
- **Process / module** → rounded rectangle (corner radius ~2–3pt; ~80% of venue figures).
- **Data / tensors** → sharp-cornered rectangle; stacks/cuboids for tensors, flat grids
  for matrices/tokens/attention maps.
- **Database / buffer / memory** → cylinder, and cylinders are used for NOTHING else.
- **Macro–micro pattern**: a small box in the global view + a zoomed breakout box
  connected by two thin lines — use instead of cramming detail into the overview.

## Arrow semantics (be strict; ambiguous arrows are a named amateur tell)
- **Solid, near-black** = main forward/data flow. One consistent direction (L→R or T→B).
- **Dashed** = auxiliary ONLY: feedback, gradients, offline paths, optional branches,
  logical grouping borders. Never dashed for decoration or "variety".
- **Orthogonal/elbow routing** for architecture precision; **gentle curves** for
  high-level system logic and feedback returns. Don't mix styles for the same role.
- Operators sit ON the line: $\oplus$ add, $\otimes$ multiply/concat.

## Status color convention (ML figures)
- Trainable / active → warm (orange, red, deep pink). Frozen / fixed → cool
  (gray, ice blue) — optionally ❄/🔥 markers if the venue tolerates icons.
- High saturation is a budget: spend it only on the loss, the ground truth, or the
  final output. Backgrounds stay at 10–15% tint (see palettes.md).

## Typography (strict, this one is binary)
- Node labels / module names → **sans-serif** (`\sffamily`), bold for headers.
- Math variables ($x$, $\theta$, $\mathcal{L}$) → **serif italic** = normal LaTeX math
  mode inside the node. "Encoder" in Times or $x$ in sans both read as 1990s.
- One family for all labels; sizes from a 2–3 step scale (e.g., \normalsize title,
  \small node, \scriptsize annotation) — not a continuum.

## Composition
- **Rectangular envelope.** LaTeX reserves the figure's bounding box; one protruding
  element wastes a text-line-height of vertical space across the full width. Keep the
  outline compact and rectangular; fill corners or move the legend into a gap.
- Left-to-right narrative: inputs left, contribution center-stage, outputs/metrics right.
- No title, no caption text, no "Figure 1:" inside the image — the LaTeX caption owns those.
- Kill redundant in-figure legends when direct labels work; a legend is a lookup cost.

## Per-domain register (match the paper's genre)
- **Agent / LLM papers**: more illustrative allowed — chat bubbles, document icons,
  simple avatars. Keep it flat 2D vector, never clip-art gradients.
- **CV / 3D papers**: geometric — camera frustums, ray lines, point clouds, RGB-coded
  axes, viridis heat overlays.
- **Theory / optimization**: minimalist textbook style — circles/nodes, manifold
  surfaces, grayscale + ONE accent color (gold or blue), no icons, no cartoons.
- **Systems / industry (ISWC-style)**: infra rails in neutral gray, service hero box,
  data assets in green family, offline/side paths dashed — see assets/template.tex.

## Named amateur tells (from the venue-eval veto list — any one can sink the figure)
- PowerPoint/draw.io defaults: heavy black outlines + preset blue/orange, background grid.
- Full sentences inside boxes (>15 words), or the method section "box-ified" verbatim.
- Raw equation dumps as nodes instead of a named conceptual block.
- Spaghetti routing, crossings, or the same arrow style for two different meanings.
- Neon / saturated primary fills; black backgrounds (auto-reject in print).
- Mixed 2D and 3D elements without a reason; mixed unrelated fonts; misaligned text.
