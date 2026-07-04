# Color palettes for paper figures

Pick ONE palette per figure (and ideally per paper — consistency across figures is a
venue-quality signal). Always check: (a) grayscale-printable, (b) colorblind-safe,
(c) meaning never encoded by red-vs-green alone — pair color with position/shape/label.

## Okabe-Ito (categorical, colorblind-safe — the Nature Methods standard)

Use for: lines/classes/methods that must be told apart. Max ~4 colors per figure.

| Role hint | Name | HTML |
|---|---|---|
| neutral / text | Black | `000000` |
| emphasis 1 | Orange | `E69F00` |
| emphasis 2 | Sky blue | `56B4E9` |
| ok / data | Bluish green | `009E73` |
| highlight bg only | Yellow | `F0E442` |
| primary | Blue | `0072B2` |
| warning / contrast | Vermillion | `D55E00` |
| secondary | Reddish purple | `CC79A7` |

```latex
\definecolor{okOrange}{HTML}{E69F00}\definecolor{okSky}{HTML}{56B4E9}
\definecolor{okGreen}{HTML}{009E73}\definecolor{okBlue}{HTML}{0072B2}
\definecolor{okVerm}{HTML}{D55E00}\definecolor{okPurple}{HTML}{CC79A7}
```

## Muted box-diagram palette (architecture/pipeline figures)

Top-venue system figures converge on **low-saturation fills + darker same-hue borders
+ near-black text**. This is the skill's default (matches `assets/template.tex`):

| Semantic role | Fill | Draw |
|---|---|---|
| core service / hero | `E9F1FB` | `2F6FB0` |
| data / assets | `E6F3EE` | `2E8B7A` |
| offline / caution / side-branch | `FBF1E3` | `BE8A2E` |
| infra / neutral rail | `F2F4F8` | `5A6472` |
| ink (text) | — | `26303A` |

NeurIPS-2025 zone-fill options (background containers, keep at 10–15% tint):
Cream `F5F5DC` (warm/academic) · Ice blue `E6F3FF` (technical) · Mint `E0F2F1`
(organic/bio) · Pale lavender `F3E5F5` (modern). Alternative for theory papers:
pure white + colored dashed borders, no fills.

ML status convention: **trainable = warm** (orange/red), **frozen = cool** (gray/ice
blue); high saturation reserved for loss / ground truth / final output only.

Rules of thumb:
- Fill ≈ 8–15% tint of the border hue (`fill=color!10` also works).
- One tint per SEMANTIC role, not per box. If two boxes share a role, share the color.
- The hero (your contribution) gets the strongest border weight (`line width=1.1pt`)
  and/or a containing `fit` box; everything else stays at 0.7pt.
- Gray out what the reader should skip (`text=gray`, dashed borders for optional paths).

## Sequential / diverging (heatmaps, intensity — rarely needed in TikZ)

Use matplotlib `viridis` (sequential) or `RdBu` (diverging, midpoint = meaningful zero)
and include as PDF instead of hand-drawing in TikZ.

## Verification (do this, don't assume)

```bash
# grayscale check: convert render and look at it
pdftoppm -r 150 -png -singlefile fig.pdf fig && convert fig.png -colorspace Gray fig_gray.png
```
Read `fig_gray.png`: every boundary that matters must survive. If two adjacent fills
merge in gray, raise the value contrast, not the saturation.
