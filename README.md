# Healer ML

Open source machine learning for disease screening and treatment research.

## Why this exists

I lost my father to lung cancer in 2010. It was found too late.
Effective treatment for early stage disease already existed. The
failure was detection.

I am not a doctor. I am an ML engineer, and detection is a problem
shaped like something I know how to work on. This project is my
attempt to use what I do know against a problem that took something
from me.

I cannot do it alone, and I do not intend to. Everything here is
public: the code, the data pipelines, the results, and the failures.
If you know ML, imaging, chemistry, statistics, or clinical
practice, there is work here for you.

## Status

Early. Lung cancer screening is the first target. See [ROADMAP.md](ROADMAP.md)
for open areas and [CONTRIBUTING.md](CONTRIBUTING.md) to get started.

## How the repo is organised

Each disease gets its own directory, and inside it the work splits into two
stages that have genuinely different inputs, metrics and failure modes:

- **`screening/`** — finding candidates. Virtual screening, docking, QSAR /
  ligand-based models, hit triage, diagnostic and biomarker classifiers.
- **`treatment/`** — what happens after a candidate exists. Lead optimisation,
  ADMET and toxicity prediction, response and resistance modelling, dosing,
  patient-stratification work.

## Layout

```
diseases/
  <disease>/
    screening/
      notebooks/   analysis notebooks (Colab-compatible)
      data/        inputs committed to the repo
      results/     run outputs — gitignored, reproducible from notebooks
    treatment/
      notebooks/ data/ results/
common/
  notebooks/       cross-disease notebooks
  scripts/         shared helpers (cheminformatics, IO, plotting)
docs/              notes, protocols, decision records
```

Directories carry `.gitkeep` files so the skeleton survives a clone even when a
disease has no work in it yet. Adding a disease means copying that same
`screening/` + `treatment/` shape.

## Diseases

| Disease | Screening | Treatment |
|---|---|---|
| [lung-cancer](diseases/lung-cancer/) | LSD1 (KDM1A) structure-based screen | — |
| breast-cancer | — | — |
| colorectal-cancer | — | — |
| prostate-cancer | — | — |
| pancreatic-cancer | — | — |
| glioblastoma | — | — |
| alzheimers | — | — |
| parkinsons | — | — |
| type-2-diabetes | — | — |
| tuberculosis | — | — |
| malaria | — | — |
| antimicrobial-resistance | — | — |

## Current work

[`diseases/lung-cancer/screening/`](diseases/lung-cancer/screening/) — AutoDock
Vina screen against the LSD1–CoREST complex (PDB 2V1D). LSD1 is overexpressed in
small-cell lung cancer; a prior ligand-based screen over ~1.1M ChEMBL/ZINC
compounds returned nothing above `p_active` 0.25, which is what motivated moving
to docking.

## Data and results

Committed under `data/`: curated inputs small enough to version (activity sets,
compound libraries). Everything a notebook produces — docked poses, checkpoints,
PDBQT files, downloaded PDB/CIF structures — goes to `results/` and is
gitignored. If a result matters, promote it to `data/` deliberately.

## Running

Notebooks are written for Google Colab (they `pip install` their own deps and
fetch the Vina release binary). Locally, expect `rdkit`, `meeko`, `gemmi`,
`pandas`, `numpy`, and AutoDock Vina 1.2.7 on `PATH`.

## Licence

Apache 2.0 — see [LICENSE](LICENSE).
