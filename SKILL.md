---
name: tikz-figure
description: |
  Draw a single publication-quality methodology / architecture / pipeline / system
  figure with TikZ + LaTeX (pdflatex), producing vector PDF + PNG + an editable .tex
  source. Use when asked for a "paper figure", "방법론 그림", method/system/architecture
  diagram, or when a Mermaid/PowerPoint draft looks cluttered, flat, or unprofessional.
version: 1.0.0
license: MIT
compatibility: claude-code
allowed-tools: Read, Write, Edit, Bash
---

# TikZ Figure — publication-quality method/architecture figures

Make ONE comprehensive, clean figure (the kind in a paper's "method overview")
in TikZ. TikZ is the publication standard: reproducible, vector, version-controllable,
and — with `positioning`/`fit`/`arrows.meta` — far cleaner than auto-layout (Mermaid,
Graphviz) or hand-coordinate PowerPoint. Render to PDF (paper) + PNG (docs) and **always
view the PNG and iterate** (2–4 passes is normal).

## When to use

- "paper / methodology / method-overview figure", "방법론 그림", architecture / pipeline / system diagram.
- A Mermaid or pptx draft is too cluttered, flat, or "not like a real paper figure".
- You need a single figure that shows the whole method, plus a vector for paper inclusion.

## 0. Preflight

```bash
command -v pdflatex >/dev/null && command -v pdftoppm >/dev/null \
  && echo "OK: texlive + poppler-utils" \
  || echo "MISSING: need 'texlive-latex-extra' (pdflatex+tikz) and 'poppler-utils' (pdftoppm)"
```

- **Use English labels.** Papers are in English, and it sidesteps CJK-in-LaTeX pain
  (`kotex`/`luatexja`/`xeCJK` are often not installed; `lualatex` may be incomplete, e.g.
  missing `luatex85.sty`). If Korean is truly required, test `lualatex` + `fontspec`
  `\setmainfont{NanumSquareRound}` first; otherwise English + a Korean caption in the doc.

## 1. Design principles (this is what makes it look paper-grade, not a flowchart)

1. **Selective content.** Nodes hold SHORT labels only. Examples, sentences, and detail
   go in the paper/doc text or tables — NOT inside the boxes. Cramming text into nodes is
   the #1 amateur tell.
2. **No crossing arrows.** Use `positioning` + `fit`; leave gaps between boxes so vertical
   arrows pass cleanly; route feedback on the margins with curves
   (`to[out=180,in=180]`). If a node below a full-width bar must reach one above it, don't
   draw a line through the bar — state the relation in the box label instead.
3. **Soft pastels, light backgrounds** (2025 venue aesthetic). Semantic colors: one tint
   per role (e.g. schema/TBox vs data/ABox vs infra vs attention). Light fill (`!8`–`!12`),
   a darker draw color. Define with `\definecolor{name}{HTML}{RRGGBB}`.
4. **Visual hierarchy.** The hero (the contribution) is bigger / bolder / filled; supporting
   pieces are lighter.
5. **Group with `fit`** containers on the background layer for subsystems; title the container.
6. **Clean arrows** via `arrows.meta` (`-{Stealth[length=2.4mm]}`). Solid = main flow,
   dashed = feedback / cross-cut. Add a small **legend**.
7. **Define styles once** (`box/.style`, `flow/.style`, …) so everything is consistent.

## 2. Start from the template

Copy `assets/template.tex` (palette + node styles + a `fit` container + a hub + a side
rail + feedback curve + legend) and adapt nodes, coordinates, and labels. It compiles as-is.

```bash
cp ~/.claude/skills/tikz-figure/assets/template.tex ./fig.tex
```

Place nodes with explicit `\node[style] (id) at (x,y) {...};` (units ≈ cm) for control, or
use `positioning` (`below=of a`) for relative flow. Wrap subsystems in a `fit` node on the
`background` layer and give it a title node.

## 3. Build & iterate (ALWAYS view the render)

```bash
pdflatex -interaction=nonstopmode -halt-on-error fig.tex >fig.log 2>&1 \
  && pdftoppm -r 180 -png fig.pdf fig && echo "rendered fig-1.png" \
  || grep -iE '^!|error' fig.log | head
```

Then **Read `fig-1.png`** and fix: overlaps, crossing arrows, uneven spacing, label
collisions, cropping. Re-render. Repeat until clean. Use `-r 200` for the final embed PNG.

## 4. Output & placement

Ship three files together:

- `name.tex` — editable source (reproducible: `pdflatex name.tex`).
- `name.pdf` — vector, drop straight into a LaTeX/Overleaf paper (`\includegraphics`).
- `name.png` — raster, embed in Markdown / docs.

Embed in a doc with a caption that links the source:

```markdown
![Method overview](path/to/name.png)
> Single method figure (TikZ). Source: [`name.tex`](path/to/name.tex) (`pdflatex`) · vector [`.pdf`](path/to/name.pdf).
```

## 5. Pitfalls (encountered → avoid)

- **Broken coordinate expressions** in `--` paths (`No shape named '([xshift=1'`) → use plain
  `(x,y)`, named anchors (`a.north`), or `(a.north -| b)`; don't nest `([xshift=..]x.y)` inside `-|`.
- **A long arrow crossing a full-width bar** always looks messy → drop it; encode the relation
  in a box label or the legend.
- **`\oplus`, `\to`, `\subseteq`** etc. need math mode: `$\oplus$` inside a node.
- **`standalone` `border=Npt`** crops tightly to content; margin curves/labels ARE content
  (included, not cut) — but verify in the render.
- **Don't name a node style after a TikZ key.** A style named `out` (or `in`) shadows the
  `to[out=..,in=..]` angle keys → `key '/tikz/out' requires a value`. Use `output`, `inp`, etc.
- **Don't over-arrow.** Fewer, well-placed arrows read better than a web of lines.
- **Background layer**: wrap `fit` containers in `\begin{scope}[on background layer] ... \end{scope}`
  so they sit behind their child nodes.

## 6. Why not the alternatives

- **Mermaid**: auto-layout, no positional control → gets messy fast; fine for quick inline overviews only.
- **Graphviz / `diagrams`**: auto-routed, tidy, but "generic" and needs `dot` installed.
- **PowerPoint / python-pptx**: common for block diagrams but hand-coordinates are fiddly and
  the result reads flat. TikZ's `fit`/`positioning`/styles give cleaner output for the same effort.
- **Web AI tools** (SciDraw, PaperBanana, Illustrae): good but require a browser/network and are
  often paid — not available headless.
