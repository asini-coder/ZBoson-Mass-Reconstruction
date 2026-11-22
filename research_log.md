# PH3043 — Z-Boson Mass Reconstruction: Research Log

**Project:** Reconstruction of the Z-Boson Mass from CERN Collision Data  
**Implementation:** Python (approved by course instructor), Jupyter Notebook  
**Dataset:** `Dimuon_SingleMu.csv`, CERN Open Data, *Events with two muons from 2011 (Primary dataset SingleMu 2011)*  
**Source:** https://opendata.cern.ch/record/5202  
**Notebook:** `notebooks/01_Data_Exploration.ipynb`

> **Documentation convention:** Distinguish completed work, verified measurements, interpretations, and next steps. Dates below are based on the notebook screenshots and user-provided progress; revise to match actual execution dates.

## Session 1 — Dataset acquisition and initial exploration (14 November 2026)

### Aim
Obtain a public CMS dimuon sample, inspect its layout, and identify the measured variables needed to reconstruct invariant mass from the two muons' four-momenta.

### Work completed
- Organized the project into `data/raw`, `data/processed`, `notebooks`, `figures`, `results`, and `references`.
- Obtained the CERN educational dimuon CSV and preserved the original dataset in `data/raw`.
- Opened the file with `pandas.read_csv()` and displayed the first five rows.
- Imported NumPy, Pandas, Matplotlib and SciPy for the planned analysis.
- Created histograms of the two muons' energies (`E1`, `E2`) and transverse momenta (`pt1`, `pt2`) with logarithmic event-count axes.

### Data provenance and scope
- CERN Open Data record: https://opendata.cern.ch/record/5202.
- Source collision data were recorded in **2011**, and the educational dataset was published in **2019**.
- This is a **derived educational subset**, not an unfiltered sample of all LHC collisions. Its selection and data limitations should be acknowledged in the final report.
- The two reconstructed muon candidates are represented by separate energy, momentum and charge columns. The dataset also supplies a precomputed `M` mass column, which will be reserved for **cross-checking**, not used to calculate our independent mass.

### Verified dataset characteristics
The following numbers were checked directly against the supplied `Dimuon_SingleMu.csv` file:

| Check | Result |
|---|---:|
| Rows (dimuon candidate events) | 83,761 |
| Columns | 21 |
| Missing cells | 0 |
| Duplicate `(Run, Event)` pairs | 0 |
| Opposite-sign muon pairs | 83,761 |
| `Q1=-1, Q2=+1` | 41,147 |
| `Q1=+1, Q2=-1` | 42,614 |
| `type1=G, type2=G` | 34,742 |
| `type1=G, type2=T` | 49,019 |

**Important:** No missing values or duplicate IDs does not guarantee all reconstructed physical quantities are valid. We will check internal kinematic consistency before making cuts or drawing conclusions. Reconstruction labels `G` and `T` require source-based definition before interpreting any differences.

### Physics learned
One row corresponds to an event containing two reconstructed muon candidates. Each muon has a measured energy, momentum components (`px`, `py`, `pz`), transverse momentum (`pt`), pseudorapidity (`eta`), azimuth (`phi`) and electric charge. To search for the Z-boson resonance, calculate the invariant mass of the pair from the **sum of their four-momenta**, event by event. Opposite charge is necessary for the target decay channel but not sufficient to establish a Z origin.

## Session 2 — Understanding histograms and extreme values (16 November 2026)

### Work completed
- Inspected the initial energy and transverse-momentum histograms.
- Noted that a few very large measurements stretched the x-axes and visually compressed most events near zero.
- Replotted `E1` and `E2` separately, each displayed only up to its own **99th percentile**. **The underlying DataFrame was not filtered**; this adjustment changed the visualization only.

### Verified descriptive statistics

| Variable | Median (GeV) | 99th percentile (GeV) | Maximum (GeV) |
|---|---:|---:|---:|
| `E1` | 27.8915 | 190.1610 | 2,377.59 |
| `E2` | 7.4191 | 127.9654 | 47,758.80 |
| `pt1` | 15.9858 | 61.1027 | 3,161.43 |
| `pt2` | 3.1752 | 58.1847 | 9,319.79 |

### Observations from the new plots
- Both energy distributions are concentrated at relatively low energies and have long tails toward higher energies.
- The visible first-muon distribution is broader than the second-muon distribution in this particular dataset.
- The 99th-percentile ranges differ (`E1` approximately 190.16 GeV, `E2` approximately 127.97 GeV); the two panels should not be compared as if they had identical axes.
- Rare extreme measurements deserve investigation before any exclusion. Their presence alone does not establish corruption, nor does a low individual muon energy exclude the event from the analysis.

### Interpretation and limitations
These are **exploratory, descriptive plots**, not evidence of a Z resonance. The dimuon invariant mass combines both muon energies **and** momenta. The distributions also reflect source event selection and reconstruction categories. We have not yet established which events contribute to a Z-boson peak.

## Research direction (provisional)

**Core objective:** Independently reconstruct the dimuon invariant-mass distribution and estimate the Z-boson resonance position with uncertainty.

**Possible individual extension:** Compare the fitted peak and its uncertainty under different histogram bin widths and physically motivated muon pseudorapidity selections. This investigates selection sensitivity and detector geometry; the educational sample does **not** support a direct measurement of intrinsic CMS detector resolution without further modeling and validation.

**Validation strategy:**
1. Compare each independently reconstructed dimuon mass with CERN's provided `M` value, accounting for rounded input data.
2. Estimate the Z peak from the distribution and compare with the accepted reference, while separating statistical uncertainty, potential bias, resolution effects and simplified-data limitations.
3. Test robustness across bin widths and, if justified by sample sizes, event-selection categories.
4. A second independent dataset is optional, not required to establish a valid baseline analysis; its different selections must be documented if used.

## Next activity — Kinematic validation before mass reconstruction

1. Verify that the reported `pt1`/`pt2` approximately agree with `sqrt(px**2 + py**2)`.
2. Inspect the range and count of any unusually large disagreements; account for rounding and reconstruction differences.
3. Check whether `E_i**2 - (px_i**2 + py_i**2 + pz_i**2)` is physically reasonable, using a documented tolerance rather than rejecting negative values due only to rounding.
4. Calculate **our own** dimuon mass from the summed energies and momenta. Check for negative invariant-mass-squared values before taking square roots.
5. Compare with `M`, then plot the mass spectrum with a Z-window view (e.g. 60–120 GeV), keeping an unfiltered baseline.

## Open questions
- What exactly do `G` and `T` denote in this version of the CMS educational data, and how does reconstruction type influence measurement quality?
- Do the most extreme energy/momentum records satisfy internal kinematic consistency checks?
- Which event-selection criteria were already applied by CERN before publishing the educational sample?
- How much do bin width and pseudorapidity cuts alter the fitted peak and its uncertainty?

## Research references to consult
- CERN Open Data: *Events with two muons from 2011 (Primary dataset SingleMu 2011)*, https://opendata.cern.ch/record/5202
- CMS Collaboration (2012), *Performance of CMS muon reconstruction in pp collision events at √s = 7 TeV*, https://doi.org/10.1088/1748-0221/7/10/P10002
- Particle Data Group, *Review of Particle Physics*, Z-boson listings, https://pdg.lbl.gov/

Kinematic consistency assessment: Transverse momentum was independently calculated from the Cartesian momentum components for both muons. The median absolute differences were approximately \(3\times10^{-5}\) GeV for both muons, indicating close agreement for most records. However, a small number of events exhibited discrepancies exceeding hundreds or thousands of GeV. These events were flagged for further investigation. No events have been removed at this stage. The next analysis will quantify the affected fraction and examine energy–momentum consistency.
---
### 5. Kinematic Consistency Assessment

**Date:** 16 November 2026

**Objective:** To verify the internal consistency of the muon momentum and energy measurements before reconstructing the dimuon invariant mass.

#### 5.1 Transverse Momentum Consistency

The transverse momentum of each muon was independently calculated from its Cartesian momentum components:

$$
p_T = \sqrt{p_x^2 + p_y^2}
$$

The calculated values were compared with the transverse momenta provided in the CERN dataset.

**Results:**

| Statistic | Muon 1 | Muon 2 |
|---|---:|---:|
| Median absolute difference (GeV) | 0.000031 | 0.000030 |
| Maximum absolute difference (GeV) | 3148.81 | 3183.06 |
| Events exceeding 0.01 GeV | 3360 | 887 |

A total of **4,124 events (4.92%)** had at least one transverse momentum discrepancy exceeding the exploratory threshold of 0.01 GeV.

Most records showed very close agreement between the calculated and reported values. However, several events exhibited large discrepancies that could not be explained by ordinary numerical rounding.

**Interpretation:** These discrepancies suggest possible inconsistencies between the reported transverse momentum and Cartesian momentum components in a subset of the educational dataset. Their causes have not yet been established.

#### 5.2 Relativistic Energy–Momentum Consistency

The energy of each muon was independently reconstructed using the relativistic energy–momentum relation:

$$
E = \sqrt{p_x^2+p_y^2+p_z^2+m_\mu^2}
$$

where the muon rest mass was taken as approximately 0.105658 GeV in natural units.

**Results:**

| Statistic | Muon 1 | Muon 2 |
|---|---:|---:|
| Mean absolute difference (GeV) | 0.0000558 | 0.0000393 |
| Median absolute difference (GeV) | 0.0000317 | 0.0000299 |
| Maximum absolute difference (GeV) | 0.00933 | 0.02939 |

The reconstructed energies agreed closely with the CERN-provided values. The differences were generally very small compared with the measured particle energies.

**Interpretation:** The energy measurements appear broadly consistent with the Cartesian momentum components and relativistic kinematics. This supports using these quantities for the initial invariant-mass reconstruction, subject to further validation.

#### 5.3 Scientific Decisions

- No events were removed during this analysis.
- The 0.01 GeV transverse momentum threshold was used for exploratory investigation, not as a final event-selection criterion.
- The cause of the large transverse momentum discrepancies remains unresolved.
- Invariant mass will be calculated independently using the recorded four-momentum components.
- The effect of potentially inconsistent events on the reconstructed Z-boson resonance will be investigated through a sensitivity analysis.

#### 5.4 Next Steps

1. Reconstruct the dimuon invariant mass from the two muon four-momenta.
2. Compare calculated masses against the CERN reference mass column.
3. Visualize the complete invariant-mass distribution.
4. Investigate whether events with inconsistent transverse momentum affect the Z-boson peak position and uncertainty.

*This is a living record. Update it only after actually running each analysis, saving its outputs, and verifying interpretations.*
