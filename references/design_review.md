# Render-and-review loop (self-critique)

After EVERY render, Read the PNG and score it BEFORE showing the user. Iterate until the
target is met or 4 iterations pass (then show best + list residuals honestly). Fix the
lowest dimension first. Two consecutive identical scores = you're polishing noise — stop.

Score 5 dimensions × 2 points. Targets: journal/conference ≥ 8.5, internal doc/README ≥ 7,
slide ≥ 6.5. **Any veto (marked ✋) caps the figure at "not shippable" regardless of score.**

## 1. Faithfulness to the manuscript (0–2) — always binds
- Every label uses the paper's CURRENT terminology, verbatim (grep the .tex/.md to check).
- ✋ Hallucination: a module/connection the method doesn't have.
- ✋ Logical contradiction: reversed flow direction, bypassed step, missing connection.
- ✋ Silent contradiction of revised text: a figure drafted against an older draft can
  assert a claim the paper retracted (e.g., drawing an offline-only variant on the
  serving path). If the paper changed since drafting, re-verify EVERY box and arrow.
- Numbers in the figure match the paper's tables to the digit, or are omitted.
- Simplification is fine and encouraged; fabrication is not. Simpler ≠ less faithful.

## 2. Conciseness / signal-to-noise (0–2)
- The figure is a visual ABSTRACTION, not the method section box-ified.
- ✋ Textual overload: full sentences or >15 words in a node (exception: a real data
  example shown as an example).
- ✋ Math dump: raw equations as nodes — name the concept, keep the equation in the paper.
- One-sentence test: state what the reader should conclude; anything not serving that
  sentence is a cut candidate.

## 3. Readability (0–2)
- ✋ Occlusion/overlap of any label or box; ✋ spaghetti/crossing arrows.
- ✋ In-figure title, caption text, or "Figure 1:" (subfigure tags (a)/(b) are fine).
- Smallest text ≥ 7pt AT FINAL SIZE (font-in-fig × final-width ÷ natural-width).
- Flow parseable at a glance: consistent direction, labels adjacent to what they name,
  no low-contrast text (light-on-light, dark-on-dark).

## 4. Layout & composition (0–2)
- Aligned rows/columns (same rank ⇒ same y), even gaps; whitespace is structure.
- Groups boxed with `fit` + titled; hero visually dominant; side branches subordinate
  (dashed / muted).
- ✋ Non-rectangular envelope: protruding elements create dead zones and waste vertical
  page space (LaTeX reserves the full bounding box).

## 5. Professional aesthetics (0–2)
- One palette, semantic colors, survives grayscale (see palettes.md; test it, don't assume).
- Consistent line widths (0.4–0.5pt normal, ~1pt hero/container); typography rules from
  style_guide.md (sans labels, math-mode variables).
- ✋ Amateur tells: PowerPoint/draw.io defaults, neon or saturated-primary fills, black
  background, chart junk (shadows/3D/gradients/background grids), decorative dashing.

## Verdict format (keep in conversation, don't ship)

```
faith 2/2 · concise 2/2 · read 1/2 (label collides with container edge) ·
layout 1/2 (row 2 uneven gaps) · aesth 2/2 → 8/10, iterate on read
```

## Grayscale check (once, before final)

```bash
pdftoppm -r 150 -png -singlefile fig.pdf fig && convert fig.png -colorspace Gray fig_gray.png
```
Read `fig_gray.png`: every boundary that matters must survive. If two adjacent fills
merge, raise value contrast, not saturation.
