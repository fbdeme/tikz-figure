---
name: tikz-figure
description: |
  Draw a single publication-quality methodology / architecture / pipeline / system
  figure with TikZ + LaTeX (pdflatex), producing vector PDF + PNG + an editable .tex
  source, with a scored self-review loop (venue-rubric critique) before shipping.
  Use when asked for a "paper figure", "방법론 그림", method/system/architecture
  diagram, or when a Mermaid/PowerPoint draft looks cluttered, flat, or unprofessional.
version: 2.0.0
license: MIT
compatibility: claude-code
allowed-tools: Read, Write, Edit, Bash
---

# TikZ Figure — publication-quality method/architecture figures

Make ONE comprehensive, clean figure (the kind in a paper's "method overview") in TikZ,
then **critique the render like a venue reviewer and iterate** before showing it.
TikZ is the publication standard: reproducible, vector, version-controllable, and fonts/
math match the paper body. Pipeline: **Spec → Draft → Render → Review loop → Ship**.

References (read the one you need, when you need it):
- `references/style_guide.md` — venue visual vocabulary: shape/arrow/color semantics,
  typography, composition, per-domain register, named amateur tells.
- `references/design_review.md` — the 5-dimension scoring rubric + veto rules for the
  render-and-review loop.
- `references/palettes.md` — Okabe-Ito, muted box-diagram palette, zone fills,
  grayscale verification.

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

## 1. Spec before drawing (5 lines, in-conversation)

Vague spec in, amateur figure out. Write down, before any TikZ:

1. **Message** — the one sentence the reader must take away. Everything else is cut candidate.
2. **Hero** — which element is the contribution (gets size/weight/containment emphasis).
3. **Flow direction** — L→R (pipelines/systems) or T→B (stacks/hierarchies); pick one.
4. **Groups & side branches** — what gets a `fit` container; what is offline/optional (dashed).
5. **Source of truth** — the .tex/.md section the labels must match. **Labels are quotes,
   not paraphrases**: grep the manuscript for each node label before drawing, and if the
   manuscript is under revision, re-verify when it changes — a figure drafted against an
   old draft can silently contradict revised claims (classic desk-review catch).

Match the domain register (agent-paper vs CV vs theory vs systems) — see
`references/style_guide.md`.

## 2. Design principles (what makes it paper-grade, not a flowchart)

1. **Selective content.** Nodes hold SHORT labels only (>15 words in a box is a venue
   veto). Examples, sentences, and equations go in the paper text — the figure is an
   abstraction, not the method section box-ified.
2. **No crossing arrows.** Use `positioning` + `fit`; leave gaps between boxes so vertical
   arrows pass cleanly; route feedback on the margins with curves (`to[out=180,in=180]`).
   If a node below a full-width bar must reach one above it, don't draw a line through
   the bar — state the relation in the box label instead.
3. **Soft pastels, light backgrounds.** Semantic colors: one tint per ROLE, shared by all
   boxes of that role. Light fill (`!8`–`!15`), darker same-hue draw color. High
   saturation is a budget spent only on the hero/output. `\definecolor` from
   `references/palettes.md`; never neon, never black backgrounds.
4. **Visual hierarchy.** The hero is bigger / bolder / contained; supporting pieces
   lighter; skippable things gray or dashed.
5. **Group with `fit`** containers on the background layer for subsystems; title the container.
6. **Semantic arrows only.** `arrows.meta` (`-{Stealth[length=2.4mm]}`). Solid = main
   flow; dashed = auxiliary (feedback/offline/optional) and nothing else. Orthogonal
   elbows for architecture, gentle curves for system logic. Operators on the line:
   `$\oplus$`, `$\otimes$`. Add a small legend when >1 arrow style.
7. **Typography is binary**: sans-serif labels (`font=\sffamily`), math-mode variables
   ($x$, $\mathcal{L}$). 2–3 font sizes max. Define styles once (`box/.style`, `flow/.style`).
8. **Rectangular envelope.** LaTeX reserves the full bounding box — one protruding
   element wastes vertical page space. Tuck legends into corners/gaps.

## 3. Start from the template

Copy `assets/template.tex` (palette + node styles + a `fit` container + a hub + a side
rail + feedback curve + legend) and adapt nodes, coordinates, and labels. It compiles as-is.

```bash
cp ~/.claude/skills/tikz-figure/assets/template.tex ./fig.tex
```

Place nodes with explicit `\node[style] (id) at (x,y) {...};` (units ≈ cm) for control, or
use `positioning` (`below=of a`) for relative flow. Wrap subsystems in a `fit` node on the
`background` layer and give it a title node.

## 4. Build & review loop (ALWAYS score the render)

```bash
pdflatex -interaction=nonstopmode -halt-on-error fig.tex >fig.log 2>&1 \
  && pdftoppm -r 180 -png fig.pdf fig && echo "rendered fig-1.png" \
  || grep -iE '^!|error' fig.log | head
```

Then **Read `fig-1.png`** and score it against `references/design_review.md`:
5 dimensions × 2 (faithfulness · conciseness · readability · layout · aesthetics),
veto rules cap at not-shippable. Target ≥ 8.5 for a paper figure, ≥ 7 for docs.
Fix the lowest dimension, re-render, re-score; stop after 4 iterations or two identical
scores. Run the grayscale check once before final. Use `-r 200` for the final embed PNG.

## 5. Output & placement

Ship three files together:

- `name.tex` — editable source (reproducible: `pdflatex name.tex`).
- `name.pdf` — vector, drop straight into a LaTeX/Overleaf paper (`\includegraphics`).
- `name.png` — raster, embed in Markdown / docs.

Paper embedding: size to the final column at design time (don't downscale a huge figure —
text shrinks with it); single-column ≈ `width=0.9\linewidth`. No title inside the figure;
the caption states the takeaway message and defines abbreviations. Keep caption terminology
identical to the figure labels and body text.

Embed in a doc with a caption that links the source:

```markdown
![Method overview](path/to/name.png)
> Single method figure (TikZ). Source: [`name.tex`](path/to/name.tex) (`pdflatex`) · vector [`.pdf`](path/to/name.pdf).
```

## 6. Pitfalls (encountered → avoid)

- **Broken coordinate expressions** in `--` paths (`No shape named '([xshift=1'`) → use plain
  `(x,y)`, named anchors (`a.north`), or `(a.north -| b)`; don't nest `([xshift=..]x.y)` inside `-|`.
- **A long arrow crossing a full-width bar** always looks messy → drop it; encode the relation
  in a box label or the legend.
- **`\oplus`, `\to`, `\subseteq`** etc. need math mode: `$\oplus$` inside a node.
- **Don't put `\\` inside a font-size group in a node.** `{\footnotesize a\\b}` breaks the
  alignment scope (`Undefined control sequence … \tikzscope@linewidth` at the node's line) →
  close the group per line: `{\footnotesize a}\\{\footnotesize b}`.
- **Plan the embed scale.** Final smallest text = font-in-fig × (embed width ÷ canvas width);
  keep it ≥ 7pt. A 20cm canvas at `0.9\linewidth` (~14.3cm) turns 7pt scriptsize into 4.9pt —
  design the canvas ≤ ~16.5cm and use `\footnotesize`+ only, or embed at `0.95\linewidth`.
- **`standalone` `border=Npt`** crops tightly to content; margin curves/labels ARE content
  (included, not cut) — but verify in the render.
- **Don't name a node style after a TikZ key.** A style named `out` (or `in`) shadows the
  `to[out=..,in=..]` angle keys → `key '/tikz/out' requires a value`. Use `output`, `inp`, etc.
- **Don't over-arrow.** Fewer, well-placed arrows read better than a web of lines.
- **Background layer**: wrap `fit` containers in `\begin{scope}[on background layer] ... \end{scope}`
  so they sit behind their child nodes.
- **Class-specific package gaps**: some venue classes pull extra packages (e.g. CEURART
  v0.6.2 → `doclicense` → `ccicons.sty`). For a local test build, stub the missing .sty in
  a scratch dir via `TEXINPUTS` — never commit the stub or a wrong-font local PDF.

## 7. Alternatives & escalation

- **Mermaid**: auto-layout, no positional control → gets messy fast; fine for quick inline overviews only.
- **Graphviz / `diagrams`**: auto-routed, tidy, but "generic" and needs `dot` installed.
- **PowerPoint / python-pptx**: hand-coordinates are fiddly and the result reads flat.
- **PaperBanana (LOCAL, headless-capable)** — installed at
  `/home/user/workspace_2026/PaperBanana/`. Use it for *illustrative/conceptual* figures
  (agent-paper narrative art, teaser figures) where raster is acceptable — it outputs
  PNG only, no vector/tex, so precise architecture diagrams stay in TikZ. Invoke:

  ```bash
  cd /home/user/workspace_2026/PaperBanana
  set -a; source .env; set +a   # API keys (gitignored)
  python skill/run.py --content-file method.txt \
    --caption "Figure 1: Overview of ..." --task diagram \
    --output /abs/path/out.png --num-candidates 1 --aspect-ratio 21:9
  ```

  Slow (~3–10 min/candidate; default `--num-candidates 10` takes 10–30 min). Its design
  knowledge (NeurIPS-2025 style guides + critic rubric) is already folded into this
  skill's `references/`.
