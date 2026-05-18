# High-Dimensional Data Analysis Workshop

A two-day graduate workshop teaching statistical methods for high-dimensional biological data ($p \gg n$). Built for the UMass Biotechnology Training Program; reusable by anyone who wants to teach a graduate-level intro to FDR control, dimension reduction, and penalized regression with AI-integrated practice.

This README is for **instructors** (you). For students: just open the published website, click the "(Colab)" link next to whatever you need to do, and follow along.

---

## What students do

There's nothing to install. Students:

1. Visit the workshop website (GitHub Pages — see *Publishing* below).
2. For lectures: read/follow the slide decks in their browser.
3. For pre-homework, homework, and labs: click **"(Colab)"** in the navbar. Each opens a Google Colab notebook prefilled with prose, code scaffolds, and answer cells.
4. `File → Save a copy in Drive` in Colab to keep their work.

The only requirements are a web browser and a Google account. No Python, no R, no Quarto.

(Optional fallback: if Colab is blocked or students prefer a different Jupyter platform — e.g., UMass Harmony — they can download the `.ipynb` files from `notebooks/` and upload them anywhere that runs Jupyter.)

---

## What you do (instructor)

### One-time setup

To **build and publish** the website, you need:

- **Quarto** ≥ 1.4 ([install](https://quarto.org/docs/get-started/))
- **Python** ≥ 3.10 with a Jupyter kernel registered as `hd-stats` (see *Troubleshooting* below if your kernel has a different name)
- **R** ≥ 4.3 (only needed if you run `make data` to regenerate CSVs from Bioconductor)
- **TinyTeX** (for PDF rendering): `quarto install tinytex`
- A clone of this repository

Then:

```bash
git clone https://github.com/pflahert/hd-stats-workshop.git
cd hd-stats-workshop
make preview                 # serve with auto-reload at http://localhost:XXXX
```

If `make preview` errors with `Jupyter kernel 'python3' not found`, see *Troubleshooting*.

### Build commands

```bash
make              # Full build: HTML site + all PDFs
make html         # Just the website (HTML)
make preview      # Live preview with auto-reload
make pdf          # Just the PDFs

make lectures-pdf # Lecture handouts as PDFs (printable articles)
make slides-pdf   # Lecture 1 slide deck as Beamer PDF
make docs-pdf     # Syllabus + assessments as PDFs

make data         # Regenerate CSVs from Bioconductor (needs ALL + multtest)
make clean        # Wipe _output/, _pdf/, .quarto/
```

### Publishing the website (GitHub Pages)

Quarto has a one-command publish:

```bash
quarto publish gh-pages
```

Run once — it sets up the `gh-pages` branch and pushes the rendered site. Subsequent runs update it. The site appears at `https://<your-github-username>.github.io/hd-stats-workshop/` (or your custom domain).

The Colab links in the navbar reference `main` on GitHub, so they automatically pick up notebook changes the moment you push to `main` — independent of when you re-publish the site.

To change the GitHub user/repo: edit the Colab URLs in `_quarto.yml` (search for `colab.research.google.com/github/pflahert/hd-stats-workshop` and replace with your path).

---

## Repository layout

```
hd-stats-workshop/
├── _quarto.yml          # Site config + navbar
├── Makefile             # Build commands
├── index.qmd            # Landing page (workshop overview + schedule)
├── README.md            # This file
├── CLAUDE.md            # Notes for Claude Code (AI dev tool) — safe to ignore
├── syllabus/
│   ├── course-design.qmd  # Pedagogical design (UbD framework)
│   └── decision-guide.qmd # Method-selection flowcharts (Mermaid)
├── lectures/
│   ├── lecture1.qmd        # Essay: "The Gene That Wasn't There" (L1 = hypothesis testing)
│   ├── lecture1-slides.qmd # Reveal.js slide deck for L1
│   ├── lecture2.qmd        # Essay: "The Histogram That Was Too Wide" (L2 = estimation)
│   ├── lecture2-slides.qmd # Reveal.js slide deck for L2
│   ├── lecture3.qmd        # Essay: "The Map That Drew Itself" (L3 = dimension reduction)
│   ├── lecture3-slides.qmd # Reveal.js slide deck for L3
│   ├── lecture4.qmd        # Essay: "Seventy Genes and a Fraud" (L4 = penalized regression)
│   ├── lecture4-slides.qmd # Reveal.js slide deck for L4
│   ├── lecture-outlines.md # Master outline at three levels of detail
│   └── images/IMAGES_TODO.md # Provenance of L1 slide images extracted from source PDFs
├── assessments/
│   ├── pre-homework.qmd # Web-readable pre-homework
│   └── homework.qmd     # Web-readable Day 1 → Day 2 homework
├── notebooks/
│   ├── pre-homework.ipynb # Colab version (Python kernel)
│   ├── homework.ipynb     # Colab version (R kernel — Q2 needs limma)
│   ├── lab1.ipynb         # Lab 1 — multiple testing
│   ├── lab2.ipynb         # Lab 2 — FDR in practice
│   ├── lab3.ipynb         # Lab 3 — PCA / UMAP
│   └── lab4.ipynb         # Lab 4 — penalized regression
└── data/
    ├── golub_expression.csv  # 3,051 genes × 72 samples
    ├── golub_metadata.csv    # 72 samples (ALL vs. AML)
    ├── all_expression.csv    # 12,624 genes × 128 samples (Bioconductor ALL)
    ├── all_metadata.csv      # 128 samples (B-cell vs. T-cell)
    ├── export_all.R          # R script that produces both CSV pairs
    └── README.md
```

---

## Distribution model

```
        ┌────────────────────────────────────┐
        │   GitHub repo (this repo)          │
        │   • source .qmd / .ipynb / data    │
        └──────────────┬─────────────────────┘
                       │
       ┌───────────────┴───────────────┐
       ▼                               ▼
  GitHub Pages                  Google Colab
  (rendered website)           (open notebook
  • lectures HTML+PDF           directly from GitHub)
  • syllabus, decision guide   • lab1 - lab4
  • homework as web reference  • pre-homework, homework
       │                               │
       └────────────┬──────────────────┘
                    ▼
                 Students
        (only need a browser + Google account)
```

The website is the front door. The Colab links in the navbar take students *directly* into the notebook without any intermediate download step. Students can also bookmark the Colab URLs for repeated access.

---

## Teaching workflow

Every lecture (L1–L4) has **both** an essay (`lectureN.qmd`) and a slide deck (`lectureN-slides.qmd`). The two are kept in lockstep: the essay is the canonical, citable form with full prose and inline figures generated from real Golub data; the slide deck is the in-class delivery form, with the same figures alongside reading-guide column layouts. Either can be read alone; together they support both reading and teaching workflows.

### Day 1 — testing then estimation

- **Lecture 1 — *The Gene That Wasn't There* (hypothesis testing).** Multiple-testing problem (Caspi 2003 / Border 2019), Golub's 3,051 tests, the $p$-value histogram diagnostic, Bonferroni / FWER, the false discovery rate, the BH procedure, and Storey's $q$-values. The dead-salmon study and the field-wide 5-HTTLPR false positive close the arc.
- **Lecture 2 — *The Histogram That Was Too Wide* (estimation).** The same histogram, reframed as an estimation problem. Stein's paradox and James–Stein shrinkage (Efron–Morris baseball); empirical Bayes; the two-groups mixture model; the local FDR as a per-test posterior; the empirical null (Singh prostate-cancer cautionary case); `limma`'s variance shrinkage as a parallel EB application.

### Day 2 — geometry then prediction

- **Lecture 3 — *The Map That Drew Itself* (dimension reduction).** PCA and the SVD, scree plots and parallel analysis, the Marchenko–Pastur / BBP detection threshold, t-SNE and UMAP, the systematic ways nonlinear embeddings mislead (UMAP-on-noise), and the PCA-first practical workflow.
- **Lecture 4 — *Seventy Genes and a Fraud* (penalized regression).** Ridge / LASSO / elastic net, the geometry of sparsity, cross-validation for $\lambda$ selection, bootstrap stability for LASSO selection, and the two case studies (MammaPrint vs. Duke metagene) that show the same statistical method producing opposite outcomes for reasons entirely about validation.

### Running a lecture

Open the essay (`lectureN.qmd`) and the slide deck (`lectureN-slides.qmd`) side-by-side. Project the slides; refer to the essay for the dense prose that the slide reading guides summarize. Press `S` in the deck to open speaker view for any speaker notes that go beyond the slide content. Each deck also renders as an article PDF (in `_pdf/`) for students who want a printable handout.

### Labs (4 × 1 hour)

Project the lab notebook in Colab. Walk through the first cells, then have students follow along on their own laptops via the navbar's "Lab N (Colab)" link. The Save-a-copy-in-Drive workflow keeps their answers persistent.

### Pre-homework

Send students the website URL plus a direct pointer to **Pre-Homework (Colab)** ahead of Day 1. Estimated time: 90 minutes.

### Day 1 → Day 2 homework

After Day 1 ends, students open **Homework (Colab)** (R kernel — they'll need to switch the runtime in Colab: `Runtime → Change runtime type → R`). Q2 uses `limma` + `qvalue`, both Bioconductor packages installed by the first cell of the notebook.

### AI integration

Every lab and both assessments include explicit AI prompts. Students are expected to:

1. **Prompt** the AI for code or explanation.
2. **Evaluate** what it returns (does it actually answer the question? are there subtle errors?).
3. **Critique** against a checklist baked into the lab.

Treat AI use as a skill, not a shortcut. Collect AI prompts as part of the homework submission; spend lecture time on common failure modes (mistaking $p$-value for $P(H_0 | \text{data})$, defaulting to Bonferroni when BH would be appropriate, calling `lm()` on $p > n$ data, etc.).

---

## Customizing for your own course

- **Different dataset**: replace `data/*.csv` with your own (genes × samples + metadata) and update the GitHub raw URLs in lab notebooks. Re-render.
- **Different anchor stories**: each lecture is built around a real published paper or scientific incident. Swap the story by editing the lecture `.qmd` — keep the same statistical concept, but use a paper/finding from your own field.
- **Different schedule**: edit `index.qmd` (the schedule tables). Times are not load-bearing on the rest of the build.
- **Add a lecture**: create `lectures/lecture5.qmd`, add it to `LECTURE_NAMES` in the `Makefile`, add a navbar entry to `_quarto.yml`.
- **Add a lab**: create `notebooks/lab5.ipynb`, add a navbar entry pointing to its Colab URL.

---

## Software requirements

| Audience | Needs |
|---|---|
| Students (in workshop) | Web browser + Google account. That's it. |
| Instructor (to build / publish) | Quarto ≥1.4, TinyTeX, Python ≥3.10 with `hd-stats` Jupyter kernel, R ≥4.3 (only for `make data`). |
| Anyone running labs locally instead of Colab | Python 3.10+ with `numpy scipy pandas scikit-learn statsmodels matplotlib seaborn umap-learn pyreadr` |
| Anyone running homework locally instead of Colab | R 4.3+ with `limma`, `qvalue` (Bioconductor) |

---

## Troubleshooting

### `make preview` errors with `Jupyter kernel 'python3' not found`

Quarto looks for a kernel named `python3` by default; this project uses `hd-stats`. Run `quarto check jupyter` and confirm what's registered. Fixes:

- **Easiest**: register your existing Python as `hd-stats` (already done if you set up this repo before):
  ```bash
  python3 -m ipykernel install --user --name hd-stats --display-name "Python (hd-stats)"
  ```
- **Alternative**: edit `_quarto.yml` and change `jupyter: hd-stats` to `jupyter: python3` (whatever name your kernel actually uses).

### `make pdf` fails with LaTeX errors

You need TinyTeX. Install once:
```bash
quarto install tinytex
```

### Lecture 1 slides have empty image placeholders

`lectures/images/` is empty in the source repo. See `lectures/images/IMAGES_TODO.md` — it lists each missing file, expected name, and what it should depict, so you can source them yourself (paper title pages, key figures, etc.). Until you supply them, `lecture1-slides.html` will render with broken-image placeholders.

### Colab "Open" link returns 404

Verify the GitHub repo is **public** and the path matches what's in `_quarto.yml`. The pattern is:
```
https://colab.research.google.com/github/<USER>/<REPO>/blob/<BRANCH>/<PATH>.ipynb
```

### `make data` fails with "no package 'ALL'"

The data export script needs Bioconductor:
```r
install.packages("BiocManager")
BiocManager::install(c("ALL", "multtest"))
```

### Stale `.quarto_ipynb*` files in `lectures/`

These are intermediate Quarto build artifacts. `make clean` doesn't remove them (they're created during render, not in `_output/`). Safe to delete:
```bash
rm lectures/*.quarto_ipynb*
```

---

## Citation / license

(Fill in: how would you like adopters to cite this material? What license?)

---

## Acknowledgments

- Golub et al. (1999) for the leukemia dataset (Zenodo: 10.5281/zenodo.8123245).
- Bioconductor `ALL` package for the B-cell/T-cell dataset.
- The replication-crisis literature (Bennett, Border, Open Science Collaboration) for the cautionary stories that anchor the lectures.
