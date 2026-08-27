# Contributing to Healer ML

Thanks for being here. This project exists because one person cannot do this
alone, so contributions of every size are wanted.

Read this before opening your first pull request. The methodology section is
not boilerplate: it is the part that makes this project worth trusting.

---

## What this project is, and is not

**Is:** open research code. Reproducible pipelines, honest evaluation, and
published results including the ones that did not work.

**Is not:** a medical device, a diagnostic tool, or clinical advice. Nothing
here is validated for patient care, and no output should be presented as
though it were. If you are writing a README or a results file, say what the
model does on a held-out set, not what it would do for a patient.

---

## Before you start

1. **Open an issue first**, or claim an existing one. This avoids two people
   building the same thing and lets us agree on scope before you spend a
   weekend on it.
2. **Small PRs get merged.** A single notebook, a single fix, a single
   documented dataset. A 3,000 line PR touching six directories will sit.
3. **Ask questions in the issue.** "I do not understand what this baseline is
   for" is a useful contribution on its own.

Look for issues tagged `good-first-issue` and `help-wanted`.

---

## Contributor License Agreement

First-time contributors are asked to sign a CLA. A bot will comment on your PR
with a link; you reply with the sign-off sentence once and never again.

Why: so the project can be relicensed or dual-licensed later without tracking
down every past contributor. Your contribution stays under the project's open
license either way.

---

## Data rules

**Never commit data files.** Not CSVs of compounds, not DICOM, not model
weights over a few MB. `data/` directories are gitignored on purpose.

Instead, add to the relevant `DATA.md`:

- where the data comes from (a URL or an access procedure)
- the exact query or filter used, if you derived it
- expected schema: column names and types
- row count and a checksum, so others can confirm they got the same thing

**Respect data use agreements.** NLST, UK Biobank, and most clinical datasets
require an individual agreement and explicitly prohibit redistribution. Do not
commit them, do not upload them to a shared drive, and do not include
patient-identifiable information of any kind in code, notebooks, issues, or
commit messages.

Public molecular data (ChEMBL, ZINC, PubChem) can be referenced by download
instruction. Still do not commit the files.

---

## Methodology standards

This is what the project is actually for. A PR that ignores these will be
asked to change, regardless of how good its numbers look.

### Splits must not leak

- **Imaging:** split by patient AND by acquisition site. Slice-level or
  study-level random splits inflate performance by several points because the
  same patient and the same scanner appear on both sides.
- **Molecules:** split by Bemis-Murcko scaffold, not by compound. Analog series
  dominate public bioactivity data, and a random split puts a molecule's cousin
  in the training set.
- **Longitudinal data:** split by time as well, when the deployment scenario is
  prospective.

### Report the baseline you beat

Every model PR must include a trivial baseline evaluated on the same split.
Marginal means, majority class, a simple logistic regression on obvious
features, whichever is appropriate. Report the delta, not only the headline
metric. A large R-squared driven entirely by two marginals is not a result.

### Calibrate, or say you did not

Raw `predict_proba` is not a probability. If a downstream decision depends on
the score, use conformal prediction, isotonic regression, or Platt scaling,
and report calibration alongside discrimination.

### Use metrics that match the decision

- Screening and detection: report PPV at a fixed sensitivity, and the number
  of unnecessary follow-ups avoided per 1,000 screens. AUC alone tells a
  clinician nothing.
- Virtual screening: report enrichment factor at 1%, not only AUC.
- Always state the class balance. Enrichment and PPV are meaningless without it.

### State the applicability domain

Say where the model is expected to work and where it is extrapolating. For
molecular models this means a similarity bound to the training set. For
imaging, the scanner types and populations represented.

---

## Negative results are first-class

If you ran an honest experiment and it did not work, that belongs in
`results/`. Include what you tried, what you measured, and why you believe the
result. This is not a consolation prize: knowing that an approach fails saves
the next contributor weeks.

The first result committed to this repository is a negative one, and that is
deliberate.

---

## Code and notebooks

- **English only** in code, comments, notebook prose, commit messages, and
  issues.
- **Notebooks:** keep outputs for results notebooks, so the finding is visible
  without running anything. Clear outputs for utility notebooks.
- **Reproducibility:** set a seed, pin versions in `requirements.txt`, and
  state runtime and hardware if a cell takes more than a few minutes.
- **Style:** PEP 8, type hints where they help. No linter is enforced yet; be
  reasonable.
- Prefer plain functions and scripts over frameworks. Someone should be able
  to read a pipeline top to bottom.

---

## Adding a new disease

Diseases are added when someone commits to working on one, not in advance.

1. Open an issue titled `New disease: <name>`.
2. Say what the specific problem is (not "Alzheimers" but "predict conversion
   from MCI to AD from baseline MRI"), what public data exists, and what the
   clinical gap is.
3. Once agreed, create `diseases/<name>/` following the existing layout:
   `screening/` and `treatment/`, each with `data/`, `notebooks/`, `results/`,
   and a `README.md` stating the problem and the metric.

Shared methodology belongs in `common/`, not copied into each disease.

---

## Pull request checklist

- [ ] Linked to an issue
- [ ] CLA signed
- [ ] No data files, no patient-identifiable information
- [ ] Split strategy stated and non-leaking
- [ ] Baseline reported alongside the model
- [ ] Metrics appropriate to the decision, class balance stated
- [ ] Notebook outputs and runtime included for results
- [ ] English throughout

---

## Conduct

Be decent. Assume good faith. Critique the method, not the person, and expect
your own method to be critiqued the same way. Rigorous review is the point
here, and it should never be unkind.

Report problems to the maintainer directly.

---

## Questions

Open a discussion. There are no stupid questions about a project that spans
machine learning, imaging, chemistry, and clinical practice, and nobody
contributing here is expert in all four. I am certainly not.
