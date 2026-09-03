# Lung cancer

## screening/

**LSD1 (KDM1A) structure-based virtual screen** —
[`screening/notebooks/lsd1_docking_v3.ipynb`](screening/notebooks/lsd1_docking_v3.ipynb)

LSD1 is a flavin-dependent demethylase overexpressed in small-cell lung cancer.
Ligand-based screening over ~1.1M ChEMBL/ZINC compounds came back empty — the
applicability domain rejected 91% of catalogue chemistry and nothing scored above
`p_active` 0.25. Docking scores against the protein structure instead, so
chemotypes unlike anything in ChEMBL are still in scope.

Pipeline: receptor prep from PDB 2V1D (FAD marks the catalytic site, box covers
the adjacent substrate channel) → RDKit ETKDG + Meeko ligand prep → AutoDock Vina
1.2.7 → validation on 20 known potent actives vs 20 random library compounds →
screen with checkpointing → shortlist filtered on ligand efficiency ≥ 0.30, one
compound per Murcko scaffold.

Inputs in [`screening/data/`](screening/data/):

| File | Rows | Columns |
|---|---|---|
| `actives.csv` | 3,944 | `smiles`, `pchembl` |
| `library.csv` | 99,990 | `smiles`, `catalog_id` |

Run outputs (`docked.csv`, PDBQT files, the 2V1D structure) land in
`screening/results/` and are not committed.

Validation gate: known actives should score roughly 1–2 kcal/mol better than
random library compounds. If they don't, the box or the receptor is wrong and
every downstream number is noise.

## treatment/

[`treatment/notebooks/lsd1_docking_v8.ipynb`](treatment/notebooks/lsd1_docking_v8.ipynb)
continues the LSD1 workflow with shortlist refinement and pose inspection for
screen hits. Lead optimisation, ADMET/toxicity, and resistance modelling for
hits that come out of the screen belong here.
