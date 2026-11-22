# PH3043 — Reconstruction of the Z-Boson Mass from CERN Dimuon Data

Undergraduate Computational Physics Lab 2 (PH3043) project, Department of Physics, University of Colombo.

> **Purpose:** Reconstruct the dimuon invariant-mass spectrum using reduced CMS educational collision datasets, compare resonance models, evaluate a simplified fitted resonance center and its uncertainties, examine detector-region dependence, and check findings against a second event sample.
>
> This is an **undergraduate analysis of reduced educational data**, not a precision measurement of the Z-boson pole mass or an assessment of CMS detector calibration.

## Main findings

- **Reconstruction:** Agreement with the dataset's stored invariant masses improved when using four-momenta reconstructed from `pT`, `eta`, and `phi`. The mean absolute discrepancy changed from **50.22 MeV** (Cartesian variables) to **11.62 MeV** (alternative reconstruction).
- **Model comparison:** A **Voigt signal + exponential background**, fitted using binned Poisson maximum likelihood, was preferred among the tested resonance models (deviance/DoF **0.973**, AIC **219.10**).
- **Baseline fitted center:** `90.89575 +0.03235 / -0.03544 GeV` (approximate 68% profile-likelihood **statistical** interval). This is **0.32%** below the **91.1879 GeV** reference used in the analysis. The result is model- and fit-range-dependent.
- **Regional comparison:** Central-minus-forward fitted-center difference: **+165.93 MeV** in SingleMu, compared with **+15.84 MeV** in the nonoverlapping DoubleMu sample. The independent dataset did not confirm a nonzero regional difference, and the difference between dataset estimates was not statistically conclusive.

These are conditional results for the selected dataset, model, fitting window and analysis choices. The full statistical/systematic uncertainty of the physical Z pole mass is **not** estimated here.

## Data sources

The two CSV files in `data/raw/` come from the **CERN Open Data Portal**, CMS educational dimuon datasets associated with **2011 proton–proton collisions at 7 TeV**:

| File | CERN source | Events in original file |
|---|---|---:|
| `data/raw/Dimuon_SingleMu.csv` | [CMS SingleMu, record 5202](https://opendata.cern.ch/record/5202) | 83,761 |
| `data/raw/Dimuon_DoubleMu.csv` | [CMS DoubleMu, record 5201](https://opendata.cern.ch/record/5201) | 100,000 |

The two reduced CSV files have 21 columns, including event identifiers, charges, momentum components and stored dimuon masses. Of the 100,000 DoubleMu records, 99 have `(Run, Event)` identifiers also present in SingleMu; they were excluded from the independent event-sample comparison (remaining DoubleMu events: 99,901). Event-level nonoverlap does not imply independent detector systematics.

Exact input-file sizes and SHA-256 hashes are archived in [`results/dataset_provenance.csv`](results/dataset_provenance.csv). **Retain the CERN record URLs and dataset attribution in any public reuse**, and check the licensing/terms on the record pages before redistributing outside this educational repository.

## Repository structure

```text
PH3043_ZBoson/
├── .gitignore
├── README.md
├── requirements.txt
├── data/
│   ├── raw/
│   │   ├── Dimuon_SingleMu.csv
│   │   └── Dimuon_DoubleMu.csv
│   └── processed/                    # optional derived files
├── notebooks/
│   ├── 01_Data_Exploration.ipynb
│   ├── 02_Invariant_Mass.ipynb
│   ├── 03_Independent_Dataset_Validation.ipynb
│   ├── 04_Closure_Tests.ipynb        # supplementary Monte Carlo work
│   └── 05_Reproducibility_Audit.ipynb
├── results/                          # exported CSV/NPY outputs
├── figures/                          # notebook-generated PNG/PDF plots
├── references/                       # optional references
├── hypothesis.md                     # supplementary simulation hypotheses
└── research_log.md
```

Additional figure exports may appear under `notebooks/`; they are preserved from the submitted project folder.

## Getting started

Use **Python 3** and run the following commands from the repository root:

```bash
python -m venv .venv
# Windows (PowerShell): .venv\Scripts\Activate.ps1
# macOS/Linux: source .venv/bin/activate
python -m pip install -r requirements.txt
python -m pip install notebook
jupyter notebook notebooks/
```

**Important:** The original notebooks assume a working directory of `PH3043_ZBoson/notebooks`. Launch Jupyter with `jupyter notebook notebooks/` from the repository root, and confirm the notebook's printed `PROJECT_ROOT` points one directory above `notebooks/`.

### Suggested run order

1. `01_Data_Exploration.ipynb` — initial SingleMu inspection and momentum reconstruction. This GitHub version fixes its file loader to explicitly choose `Dimuon_SingleMu.csv` even when both raw files are present.
2. `02_Invariant_Mass.ipynb` — mass histograms, fitted models, fit diagnostics, likelihood intervals, regional fits and bootstrap resampling. This notebook loads `results/dimuon_reconstructed.csv` produced by Notebook 01; a saved copy is provided.
3. `03_Independent_Dataset_Validation.ipynb` — DoubleMu overlap removal, region comparisons and independent bootstrap analysis. Uses output(s) from prior notebooks.
4. `05_Reproducibility_Audit.ipynb` — dataset provenance, cutflow and physics consistency checks.
5. `04_Closure_Tests.ipynb` — **optional/supplementary** simulation experiments; not part of the condensed presentation or main technical report.

Some notebooks contain exploratory and repeated cells. Run them **in order in a fresh kernel**, and consult notebook Markdown and [`research_log.md`](research_log.md) to identify baseline versus sensitivity analyses. The included `figures/` and `results/` are existing outputs, not independent validation of a fresh end-to-end rerun.

## Core physics and statistical method

With natural units (`c = 1`), the pair invariant mass is

```text
M_mumu^2 = (E_1 + E_2)^2 - |p_1 + p_2|^2
```

For the alternative reconstruction of each muon's four-momentum:

```text
px = pT * cos(phi)
py = pT * sin(phi)
pz = pT * sinh(eta)
E  = sqrt(pT^2 * cosh(eta)^2 + m_mu^2)
```

The principal resonance fit uses **85–97.5 GeV**, nominal **0.5 GeV** binning, and an integrated **Voigt + exponential-background** expectation fitted to Poisson bin counts. Region categories are **central–central** (both `|eta| < 1.2`) and **forward-containing** (at least one `|eta| >= 1.2`). Robustness checks vary histogram binning, fit window and momentum-consistency criteria.

## Reproducibility notes

- Raw CERN CSVs are preserved as supplied in the archive. SHA-256 hashes are included in `results/dataset_provenance.csv`.
- Checked event-level overlap is removed from DoubleMu only for cross-dataset replication, not by mutating the original raw CSV.
- The notebook environments are recorded in `requirements.txt` as originally supplied. Exact compatibility depends on your local Python version; a clean environment is recommended.
- **Reconstruction agreement with the stored CERN mass is not an independent calibration against the physical Z pole mass.**
- Approximate confidence intervals shown in this repository primarily describe **statistical** uncertainty under the selected fit and do not cover full detector-response/model systematic effects.

## Citation and attribution

Please cite the original CERN Open Data records when using the data:

- CMS Collaboration. *Dimuon spectrum, SingleMu primary dataset* (CERN Open Data record [5202](https://opendata.cern.ch/record/5202)).
- CMS Collaboration. *Dimuon spectrum, DoubleMu primary dataset* (CERN Open Data record [5201](https://opendata.cern.ch/record/5201)).

For fitting software, this project uses SciPy's [`scipy.special.voigt_profile`](https://docs.scipy.org/doc/scipy/reference/generated/scipy.special.voigt_profile.html) and SciPy optimization routines; additional literature and full citations are provided in the associated technical report.

## Authorship and usage

Created for undergraduate coursework. This repository preserves both the primary analysis and optional exploratory investigations. See the individual notebooks and research log for detailed decisions and limitations. If AI-assisted writing or code development must be disclosed, follow your institution's submission requirements.

**License note:** No license has been granted here for the original student-authored code. Add a deliberate code license only if the author wishes to grant reuse rights. CERN data and third-party materials retain their own terms.
