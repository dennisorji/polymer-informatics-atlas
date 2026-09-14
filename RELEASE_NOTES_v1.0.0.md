# Polymer Informatics Atlas v1.0.0

This is the first complete public research release of the **Polymer Informatics Atlas**.

## Scope

The release contains a six-notebook polymer-informatics workflow spanning:

- chemical data auditing and target-availability analysis;
- polymer-specific molecular representation design;
- property-specific predictive modelling with frozen holdouts;
- chemistry-aware generalization stress tests;
- masked multitask learning for incomplete labels;
- neural-ensemble disagreement and split-conformal prediction;
- chemically cautious decoding of important Morgan environments;
- an integrated unsupervised Polymer Atlas;
- representative property archetypes and an illustrative multi-objective design analysis.

## Dataset and targets

The workflow was developed around 7,973 unique, RDKit-valid polymer repeat-unit records from the NeurIPS Open Polymer Prediction 2025 Kaggle dataset.

Observed target counts are:

- Tg: 511
- FFV: 7,030
- Tc: 737
- Density: 613
- Rg: 614

The raw competition dataset is not redistributed in this release.

## Final random-holdout performance

| Target | Model / representation | R² |
|---|---|---:|
| Tg | Extra Trees / Hybrid | 0.608 |
| FFV | HistGradientBoosting / Hybrid | 0.704 |
| Tc | HistGradientBoosting / Hybrid | 0.825 |
| Density | Ridge / Interpretable | 0.920 |
| Rg | HistGradientBoosting / Morgan r4 | 0.703 |

## Key scientific findings

- Polymer-property prediction is property specific in both useful representation and model behaviour.
- Density is captured strongly by global/compositional descriptors, whereas Tc and Rg depend much more strongly on local/topological structure.
- Chemistry-separated evaluation materially changes the interpretation of model quality; Tc shows the clearest generalization collapse.
- Masked multitask learning uses partially labelled polymers more effectively than complete-case multitask learning, but positive transfer over matched single-task neural models is small and seed dependent.
- Reliability is multidimensional: chemical-domain support, ensemble disagreement, and conformal calibration provide different information.
- Fixed 90% split-conformal intervals achieved approximately 95.7% empirical coverage for Tc, 92.3% for Density, and 98.5% for Rg.
- Twenty-eight important Morgan indices were chemically decoded after exact fingerprint-regeneration checks; 261,846 selected-bit occurrences were recovered with zero count mismatches.
- The final Atlas maps all 7,973 polymers in an unsupervised integrated descriptor/fingerprint space and supports property, error, domain-support, and uncertainty overlays.

## Reproducibility and claim boundaries

The repository contains all six executed notebooks, release-facing result tables, compact summary figures, citation metadata, and a dedicated reproducibility protocol.

Large intermediate arrays, fitted model objects, neural weights, notebook-output ZIP archives, and the source competition data are intentionally excluded from Git history.

The project reports statistical structure–property relationships and predictive dependencies. It does not claim experimentally proven causal polymer-design laws or unrestricted extrapolation to arbitrary polymer chemistry.

## Citation

Citation metadata are provided in `CITATION.cff`. A persistent Zenodo DOI will be added to the repository after this release is archived through the Zenodo–GitHub integration.
