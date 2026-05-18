# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Two-day graduate workshop (May 2026) teaching statistical methods for high-dimensional biological data (p >> n). Covers multiple testing/FDR control, dimension reduction, and penalized regression, with AI-integrated learning and critical evaluation.

## Build Commands

```bash
make              # Full build (HTML + PDF)
make html         # Render full website
make preview      # Live preview with auto-reload

make lectures     # Render lecture notes / slide decks as HTML
make slides       # Render lecture1-slides as Reveal.js HTML
make docs         # Render syllabus, assessments, decision guide as HTML

make pdf          # Render all PDFs
make lectures-pdf # Lecture notes/decks as PDF (printable handouts)
make slides-pdf   # lecture1-slides as Beamer PDF
make docs-pdf     # Syllabus/assessments as PDF

make data         # Export Golub + ALL dataset CSVs (requires Bioconductor)

make clean        # Remove all output
make clean-pdf    # Remove PDF output only
```

Primary tool is Quarto (≥1.4). Output goes to `_output/`. PDFs go to `_pdf/`. Local Jupyter kernel for Python execution is named `hd-stats` (set via `jupyter: hd-stats` in `_quarto.yml`).

## Directory Structure

```
hd-stats-workshop/
├── _quarto.yml           # Quarto website config, navbar, project-level jupyter kernel
├── CLAUDE.md
├── README.md             # Instructor-facing teaching guide
├── Makefile
├── index.qmd             # Syllabus / landing page
├── syllabus/
│   ├── course-design.qmd # UbD framework (enduring understandings, assessment design, lecture plan)
│   └── decision-guide.qmd # Mermaid flowcharts for method selection
├── lectures/
│   ├── images/           # Slide image assets (see IMAGES_TODO.md for missing files)
│   ├── lecture-outlines.md # Master outline at three levels of detail
│   ├── lecture1.qmd      # Essay: "The Gene That Wasn't There" (HTML article + PDF)
│   ├── lecture1-slides.qmd # Reveal.js slide deck for Lecture 1 (image-driven)
│   ├── lecture2.qmd      # Essay: "The Histogram That Was Too Wide" (HTML article + PDF)
│   ├── lecture2-slides.qmd # Reveal.js slide deck for Lecture 2
│   ├── lecture3.qmd      # Essay: "The Map That Drew Itself" (HTML article + PDF)
│   ├── lecture3-slides.qmd # Reveal.js slide deck for Lecture 3
│   ├── lecture4.qmd      # Essay: "Seventy Genes and a Fraud" (HTML article + PDF)
│   └── lecture4-slides.qmd # Reveal.js slide deck for Lecture 4
├── assessments/
│   ├── pre-homework.qmd  # HTML/PDF web reference for pre-workshop assignment
│   └── homework.qmd      # HTML/PDF web reference for Day 1→Day 2 assignment
├── notebooks/
│   ├── pre-homework.ipynb # Colab assignment (Python kernel)
│   ├── homework.ipynb     # Colab assignment (R kernel — uses limma + qvalue)
│   ├── lab1.ipynb        # Lab 1: The Multiple Testing Problem
│   ├── lab2.ipynb        # Lab 2: FDR in Practice
│   ├── lab3.ipynb        # Lab 3: Dimension Reduction
│   └── lab4.ipynb        # Lab 4: Penalized Regression
└── data/
    ├── golub_expression.csv # 3,051 genes × 72 samples (Golub 1999, ALL vs. AML)
    ├── golub_metadata.csv   # 72 samples with class labels
    ├── all_expression.csv   # 12,624 genes × 128 samples (Bioconductor ALL)
    ├── all_metadata.csv     # 128 samples with B-cell / T-cell labels
    ├── export_all.R         # Exports both Golub and full ALL CSVs
    └── README.md            # Data documentation
```

## Content Architecture

Every lecture has **both** an essay and a slide deck:

- **Essays** (`lecture{1,2,3,4}.qmd`): HTML article + PDF, classic-academic prose with inline DOI links to primary sources. All essays now contain **executable Python code blocks** that generate figures inline (both simulation and real-data, e.g., Golub) via the `hd-stats` kernel. The essay is the canonical, citable, figure-rich form.
- **Slide decks** (`lecture{1,2,3,4}-slides.qmd`): Reveal.js. Every image- or math-only slide is wrapped in a two-column layout with a reading-guide column. All slide decks use the inline CSS block (body 0.78em, h1 2.4em, h2 1.5em — see "Slide CSS" below).

**Lecture 1 = hypothesis testing**; **Lecture 2 = estimation.** Day 1 thus has two complementary lectures (deciding which genes to call significant; estimating quantities from the same histogram). BH and Storey's $q$-values live in L1; Stein/JS/empirical-Bayes/two-groups/local FDR/empirical null live in L2. Day 2 continues the estimation theme (L3 = dimension reduction; L4 = penalized regression).

**Assessments and labs** follow a dual-form pattern:
- `assessments/*.qmd` — readable HTML/PDF web reference.
- `notebooks/*.ipynb` — interactive Colab version where students actually do the work. Linked from the navbar with a "(Colab)" suffix and from the top of each `.qmd` via a callout box.

**Labs** (`notebooks/lab1.ipynb`–`lab4.ipynb`): Python Colab notebooks; zero-install for students; structure: setup → write code → use AI → critique checklist.

### Learning Outcomes

- **LO1**: Multiple testing, FDR control, empirical Bayes
- **LO2**: Dimension reduction (PCA/UMAP), penalized regression (ridge/LASSO/elastic net)
- **LO3**: Critical evaluation of analysis code and AI-generated output

## Style and Pedagogical Conventions

The following conventions were established by iterative review; preserve them across future edits.

### Lecture essays

- **Open every lecture with an `## Orientation` paragraph** (~5–8 sentences) that names: the question the lecture answers, the methods developed, the recurring example, and the bridge to the next lecture. The essay arc is then anchored by this paragraph.
- **Anchor with a primary paper, not an aphorism.** L1 = Caspi 2003 (5-HTTLPR) and Border 2019; L2 = Stein 1956 / Efron–Morris 1977 / Singh 2002; L3 = Turk-Pentland eigenfaces; L4 = the MammaPrint / Duke "metagene" cases. Link to DOIs inline.
- **Use the Golub data as the recurring concrete example** across L1–L2 and Labs 1–2. Where a figure can be generated from Golub directly, prefer that to simulation.
- **Figures are abundant and inline.** Generate them with `{python}` blocks that read from `data/golub_*.csv` (essays live in `lectures/`, so use `../data/golub_*.csv`). Mix simulation (for paradoxes, what-if curves) with real Golub data (for concrete results). Use `#| echo: false` so the essay reads as prose with figures, not as code.
- **Every figure has a `#| fig-cap`** explaining what to look at. The caption is the figure's reading guide; prose around the figure draws the conclusion.
- **matplotlib mathtext quirk:** `\boldsymbol` is not supported in figure labels/titles — use `\theta` directly. Markdown-level math via MathJax handles `\boldsymbol` fine.
- **Cross-link lectures explicitly** in the closing paragraph (L1 → L2 = testing → estimation; L2 → Day 2 = continued estimation).

### Slide decks

- **No punchy aphorisms.** Avoid single-sentence centered slides like "The framework exists." or "Same biology. Different results." Replace with academic prose or denser content. The exception: multi-item instructional `. . .` reveals (BH equation → theorem; FWER vs. FDR contrast; three reinforcing mechanisms) are pedagogically useful and may stay.
- **Every image-only or math-only slide gets a two-column layout** (`:::: {.columns}` + `::: {.column}` × 2) with a reading-guide column. Pattern: image/figure on left (55–62%), reading guide on right (45–38%). The reading guide answers: what claim is being made, how to read the axes/labels, what the takeaway is.
- **Slide titles state the claim.** Not "The 8,000-citations slide" but "A field-defining finding — and an inconsistent replication record." Not "Bonferroni correction" but "Bonferroni: divide the per-test threshold by $m$."
- **Section dividers (`# Title {.centered}`) need a real h1 size.** With body shrunk to 0.78em (see CSS below), a default h1 looks like body text — restore it explicitly.
- **Slide CSS block.** Paste this immediately after the YAML in every slide deck so font sizes match across lectures:
  ```html
  ```{=html}
  <style>
  .reveal .slides { font-size: 0.78em; }
  .reveal h1 { font-size: 2.4em; }
  .reveal h2 { font-size: 1.5em; }
  .reveal table { font-size: 0.9em; }
  .reveal blockquote { font-size: 0.95em; }
  </style>
  ```
  ```
- **Speaker notes** (`::: {.notes}`) describe *delivery* — what to emphasize, what to pause on, what students often miss. They do not duplicate slide text. Use them sparingly.
- **Empty `## {.centered}` slides read as blank.** Always give the slide a real title; if the slide is content-only, name the content.
- **Slides mirror essay structure.** When the essay sections change order or get new sections, push the same shape into the slides. Don't let them drift apart.

### Worked examples (figures that earn their keep)

- **James–Stein shrinkage figure.** Place MLE and JS estimates on a shared *value* axis (not sequential positions) so the spread of each row is visually meaningful, and draw slanted arrows connecting paired estimates. Annotate $\bar y$ as a vertical line and report SD(MLE) and SD(JS) in an inset. Visualizes shrinkage as motion toward the grand mean and as spread reduction.
- **Two-groups mixture.** Show three filled regions: $\pi_0 f_0(z)$, $(1-\pi_0) f_1(z)$, and their sum $f(z)$. The viewer should *see* the decomposition.
- **Local FDR curve.** Plot $\widehat{\mathrm{fdr}}(z)$ from -6 to 6, with the 0.20 (or 0.10) threshold marked and the $z$-crossings annotated. Pair with the histogram above it when possible.
- **Discovery-count bar chart.** When comparing Bonferroni / BH / local FDR (or any three methods) on the same dataset, a labeled bar chart with the count on each bar is the cleanest summary.
- **PDF page extraction for figures from papers.** Use `pdftoppm -r 250 -f $page -l $page -x $x -y $y -W $w -H $h -png input.pdf out` for precise region cropping. Avoid `sips --cropOffset` — its offset semantics are easy to get wrong.

### Labs

- **Lab intro** explicitly names the lectures the lab applies. Example: "From Lecture 1 (testing): BH and Storey's $q$-values. From Lecture 2 (estimation): two-groups, local FDR, empirical null."
- **AI prompts in labs must lead with a format directive.** The UMass campus GenAI platform (LibreChat) will return a `.docx` for long answers unless told otherwise. The prompt template:
  > "Reply with a single Python code block (no prose, no `.docx` file, no surrounding explanation — just runnable code I can paste into a Colab cell). [Then the actual prompt.]"
- **Name the imports the AI should use.** If the prompt expects `statsmodels.stats.multitest.multipletests` with `method='fdr_bh'`, say so. Reduces variation in student outputs and makes the critique-checklist step more concrete.
- **Critique checklists are part of the lab.** After every AI-assisted code cell, students answer a small table of "did the AI do X correctly?" The checklist is the LO3 evidence.

### Resources and copyrighted material

- **`resources/papers/` is gitignored.** Source PDFs of journal articles are archived locally for instructor reference (e.g., for extracting figures or quoting). Do not commit them — the public GitHub repo would expose them, which is a copyright issue. The figure *extracts* (single figures used for educational/classroom purposes) are committed in `lectures/images/` under fair use.
- **`lectures/images/IMAGES_TODO.md`** records the provenance of each extracted figure, pointing back to the archived source PDF.

### Workflow and infrastructure

- **`hd-stats` Jupyter kernel cold-start is slow** (~30 seconds). The render appears to "stall" but is just waiting for the kernel. Don't kill it; wait. Subsequent renders in the same session are fast.
- **`quarto publish gh-pages` re-renders the whole site** (4 lectures, 4 slide decks, all labs and assessments). Budget ~5 minutes per deploy.
- **A "blank" gh-pages worktree at `/private/tmp/hd-stats-ghpages`** can be left behind by an interrupted publish. Run `git worktree prune` to clear it before retrying.
- **Verify deploys against the live URL.** GitHub Pages caches aggressively; a hard refresh (Cmd+Shift+R) is often needed to see the deployed changes.

## Code Conventions

- **Lecture essays** (`lecture{1,2,3,4}.qmd`): pure prose, no executable code; cite primary sources inline as `[Author (year)](https://doi.org/...)`.
- **Lecture 1 slides** (`lecture1-slides.qmd`): image-driven; `code-fold: true` in Reveal.js settings. No live code execution.
- **Lectures 2–4 slides** (`lecture{2,3,4}-slides.qmd`): mix of on-slide narrative + executable Python (`{python}` blocks with `echo: true`). Figures render live during build via the `hd-stats` Jupyter kernel.
- **Labs** (Python notebooks): `numpy.random.seed(2026)` for reproducibility; `plt.style.use('seaborn-v0_8-whitegrid')` for plots; data loaded from Zenodo or GitHub raw URLs.
- **Homework notebook** (R kernel): uses `limma` + `qvalue` (Bioconductor); data loaded from GitHub raw URLs.

## Key Datasets and Packages

- **Golub dataset**: 3,051 genes × 72 samples, ALL vs. AML leukemia.
  - Source: Golub et al. (1999), *Science*; archived on Zenodo (DOI: 10.5281/zenodo.8123245).
  - Local exports: `data/golub_expression.csv`, `data/golub_metadata.csv`.
  - Loaded by labs from GitHub raw URLs: `https://raw.githubusercontent.com/pflahert/hd-stats-workshop/main/data/`.
- **ALL dataset**: 12,624 genes × 128 samples, B-cell vs. T-cell ALL.
  - Source: Bioconductor `ALL` package.
  - Local exports: `data/all_expression.csv`, `data/all_metadata.csv`.
- Both produced by `data/export_all.R` (run via `make data`; requires Bioconductor `ALL` + `multtest`).
- **Python** (labs, lectures 2–4): scipy, statsmodels, scikit-learn, umap-learn, matplotlib, seaborn, pandas, numpy.
- **R** (homework, optional local): limma, qvalue, glmnet, tidyverse, ggplot2, pheatmap, Rtsne, uwot.

## AI Integration Philosophy

Every lab uses AI explicitly. Students prompt AI, evaluate output, and catch errors. Labs include deliberately subtle errors for students to identify. Assessment focuses on understanding what AI produces, not just using it.

## Editing Guidelines

- For every lecture: the essay (`lecture{N}.qmd`) is the canonical text and cites primary sources. The slide deck (`lecture{N}-slides.qmd`) is the in-class delivery form. When the science changes, update the essay first; sync the slides afterwards.
- For assessments: edit the `.qmd` and the corresponding `.ipynb` in lockstep — they should stay in sync.
- Preserve narrative flow — each lecture tells a story with an anchor paper/finding.
- Keep visual emphasis (p-value histograms, scree plots, biplots).
- Slide image placeholders use `lectures/images/` — see `IMAGES_TODO.md` for the current list of missing files.
- Update `_quarto.yml` navbar if adding new documents (and add a Colab link if the new doc has a paired `.ipynb`).
- Decision guide uses Mermaid flowcharts — test rendering after edits.
- Slides have speaker notes accessible via Reveal.js speaker view (press 'S').
- Do not commit `_output/`, `.quarto/`, `_pdf/`, or `*.quarto_ipynb*` artifacts.

## Distribution model

- **Source on GitHub**: `pflahert/hd-stats-workshop`.
- **Website**: served from GitHub Pages via `quarto publish gh-pages`.
- **Labs and assessments**: distributed as `.ipynb` opened by students directly in **Google Colab** (URLs of the form `colab.research.google.com/github/pflahert/hd-stats-workshop/blob/main/notebooks/<file>.ipynb`).
- **Alternative**: students may upload the same `.ipynb` files to UMass Harmony or any other Jupyter-capable platform.
