
# PH3043 — Preregistered Simulation Hypotheses

## Research question

How reliably can a simplified Voigt-plus-background fitting
pipeline identify a regional Z-boson resonance-center difference
in finite CMS-like dimuon samples?

## Baseline method

- Signal: Voigt profile
- Background: Exponential
- Fit: Binned Poisson maximum likelihood
- Mass window: 85–97.5 GeV
- Bin width: 0.5 GeV
- Detector categories: Central-central and forward-containing
- Random seed: 3043
- Nominal Lorentzian HWHM: 1.24775 GeV

The generator will use event counts and nuisance-parameter
values based on the previously fitted samples.

Simulation settings will be recorded before generating results.

## H1 — Null hypothesis / false-positive calibration

True regional center difference: 0 MeV.

H0: The two simulated regions share the same center.

Objective:
Measure the fraction of simulated experiments that reject
the common-center hypothesis at significance level 0.05.

An approximately calibrated test should have a rejection
rate near 5%, allowing for Monte Carlo uncertainty.

## H2 — Statistical power

Injected regional center difference: 166 MeV.

Objective:
Estimate the probability of detecting this shift using
SingleMu-like and DoubleMu-like event counts.

Compare detection power and estimator precision.

Do not assume beforehand that either dataset must detect
the injected difference.

## H3 — Estimator bias and interval coverage

Generate samples with known resonance centers.

Calculate:
- Mean fitted-center bias
- Root-mean-square error
- Standard deviation of fitted centers
- Coverage of nominal 68% confidence intervals

Expected coverage under a calibrated method is
approximately 68%, within simulation uncertainty.

## H4 — Model misspecification (optional)

Generate data with a modified background or
resolution model but fit using the baseline model.

Determine whether:
- Estimated centers become biased
- False-positive rates increase
- Confidence-interval coverage deteriorates

## Interpretation rules

1. Report all attempted simulation replicates.
2. Record optimization failures and parameter-bound cases.
3. Quantify Monte Carlo uncertainty.
4. Do not discard inconvenient results.
5. Distinguish statistical bias from model-dependent bias.
6. Do not interpret simulated effects as evidence of
   actual CMS detector miscalibration.
7. Avoid claiming novel particle-physics discoveries.
``