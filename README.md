# CRTs: Methods for Reach and Impact

Quarto website for Aim 3 of the R01 — dissemination of Two-Stage TMLE methods
for cluster randomized trials (CRTs).

## What this is

A training site for researchers analysing CRTs of group-level health
strategies — particularly strategies that aim to both *reach* a target
population and *improve outcomes* among those reached (e.g., HIV prevention
trials). Anchored on the Causal Roadmap; powered by the
[`TwoStageTMLE`](https://github.com/LauraBalzer/TwoStageTMLE) R package.

## Year-1 scope

Chapter stubs for sections 0–7 of the eventual site, with two working R
hands-on chapters that exercise the package end-to-end:

- **Chapter 5** — foundational Two-Stage TMLE (Balzer et al. 2021): handles
  missing outcomes via per-cluster TMLE on `(Δ, W, M)`.
- **Chapter 6** — mediated Stage 1 (Nakato & Balzer 2026): handles the
  multilevel-mediation-missing data problem via the ratio endpoint
  `Yc = E[Y2] / E[P(Y1 = 1 | M = 1, W)]`.

Information chapters (1–4) are stubs that lay out the Causal Roadmap, the
methodological challenges of CRTs, and the conceptual basis of Two-Stage
TMLE. Year 2 will expand chapter 5 → effect heterogeneity (Aim 1B) and
add the Shiny "Roadmap walker" embedded as an iframe.

## Build

Quarto and R must be installed; the package must be installed too:

```sh
# 1. Install the TwoStageTMLE dev package
R -e 'remotes::install_local("../two_stage_tmle_claude-code/TwoStageTMLE")'

# 2. Preview locally (re-renders on save)
quarto preview

# 3. Build the static site into ./docs (local preview)
quarto render
```

## webR (in-browser R)

The site is wired up with [webR](https://docs.r-wasm.org/webr/) via the
[`coatless/quarto-webr`](https://github.com/coatless/quarto-webr)
extension. Cells marked `{webr-r}` are interactive — they execute in
the reader's browser using a WebAssembly build of R, with no server
component.

Packages pre-loaded on page load are configured under `webr.packages`
in `_quarto.yml`. They must be available either in the
[default webR repo](https://repo.r-wasm.org/) or in a wasm-built
[r-universe](https://r-universe.dev/) binary.

### TwoStageTMLE wasm binary — TODO

Right now `TwoStageTMLE` is not wasm-published, so the chapter-5 and
chapter-6 cells that *call* the package still render server-side at
build time. Once the package is on r-universe (e.g.,
`LauraBalzer.r-universe.dev`) with wasm binaries enabled, the next
steps are:

1. Add the universe to `webr.repos` in `_quarto.yml`:
   ```yaml
   webr:
     repos:
       - https://laurabalzer.r-universe.dev
     packages:
       - data.table
       - ltmle
       - TwoStageTMLE
   ```
2. Switch the relevant `{r}` chunks in `05_R_foundational.qmd` and
   `06_R_mediated.qmd` to `{webr-r}`.

The site already renders the webR demo on chapter 5 against `data.table`
only — that proves the pipeline end-to-end and is informative on its own.

## Publish to Quarto Pub

For sharing drafts (e.g., with the External Review Board):

```sh
# First time: creates _publish.yml tracking the target URL
quarto publish quarto-pub
```

Subsequent publishes push to the same URL recorded in `_publish.yml`.
The rendered output is *not* committed — Quarto Pub hosts it.

## Structure

```
_quarto.yml        # site config, sidebar, theme
index.qmd          # welcome
00_instructors.qmd # team
01_..04_info_*.qmd # information chapters (stubs)
05_..06_R_*.qmd    # hands-on R chapters (live code against the package)
11_info_*.qmd      # closing remarks
references.bib     # bibliography
styles.css         # CSS overrides
images/            # figures (place .png/.pdf here)
data/              # sample datasets (kept small)
helpers/           # optional custom Lua / HTML / CSL
```

## License

Content: CC-BY-4.0. Code snippets: GPL-3 (matching the underlying R package).
