# tikz-figure

A Claude Code skill: draw a **single publication-quality methodology / architecture /
pipeline figure** with **TikZ + LaTeX** (the kind in a paper's "method overview"). Produces
a vector **PDF** (drop into a paper), a **PNG** (embed in docs), and an editable **`.tex`**
source. Reach for it when a Mermaid or PowerPoint draft looks cluttered, flat, or "not like
a real paper figure".

## Install

```bash
mkdir -p ~/.claude/skills/tikz-figure
curl -sL https://github.com/fbdeme/tikz-figure/archive/main.tar.gz \
  | tar xz -C ~/.claude/skills/tikz-figure --strip-components=1
```

## Use

Invoke `/tikz-figure`, or just ask for a *"paper figure / 방법론 그림 / architecture diagram"*.
The skill:

1. **Preflight** — checks `pdflatex` + `pdftoppm`.
2. **Start from `assets/template.tex`** — palette, node styles, a `fit` container, a hub bar,
   a side rail, a feedback curve, and a legend (compiles as-is).
3. **Apply design principles** — selective short labels (detail goes in text/tables), soft
   pastel palette, **no crossing arrows**, `positioning`/`fit`, clear hierarchy, legend.
4. **Build & iterate** — `pdflatex` → `pdftoppm` → **view the PNG** → fix → repeat.
5. **Ship** — `.tex` + `.pdf` + `.png`, embedded with a caption that links the source.

## Requirements

- TeX Live with TikZ (`texlive-latex-extra`) and `poppler-utils` (`pdftoppm`).
- **English labels recommended** — papers are in English, and it avoids CJK-in-LaTeX pain
  (`kotex`/`luatexja` are often missing). Put any localized prose in the doc caption.

## What it looks like

The bundled template (`assets/template.tex`) renders a generic skeleton you adapt:

![template preview](assets/template-preview.png)

See [`SKILL.md`](SKILL.md) for the full workflow, design principles, and pitfalls.

## License

MIT
