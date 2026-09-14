# Polymer Informatics Atlas

A chemistry-aware polymer informatics study integrating structure–property modelling, chemical-domain generalization, multitask learning, uncertainty quantification, and interpretable polymer design.

## Overview

The **Polymer Informatics Atlas** is a six-notebook research workflow built around 7,973 polymer repeat-unit records from the NeurIPS Open Polymer Prediction 2025 dataset. The study progresses from chemical data auditing and representation design to property-specific modelling, chemistry-aware stress testing, shared learning, calibrated reliability, and an integrated structure–property atlas.

The project is designed as a research study rather than a leaderboard exercise. Representation choice, interpolation performance, chemical-domain generalization, transfer behaviour, uncertainty, and final chemical interpretation are evaluated as separate scientific questions.

## Research questions

- Which polymer representations preserve chemically meaningful repeat-unit information?
- Which properties are captured by global interpretable descriptors versus local/topological Morgan environments?
- How much does conventional random-split performance differ from chemically separated generalization?
- Can partially labelled polymer records improve shared learning?
- Can prediction-level disagreement and chemical-domain support identify unreliable predictions?
- Which structural motifs and cross-property trade-offs emerge from the final models?

## Dataset

Core analysis population: **7,973 unique, RDKit-valid polymer repeat units**.

| Property | Meaning | Unit | Observed records |
|---|---|---|---:|
| Tg | Glass-transition temperature | °C | 511 |
| FFV | Fractional free volume | dimensionless | 7,030 |
| Tc | Thermal conductivity | W m⁻¹ K⁻¹ | 737 |
| Density | Polymer density | g cm⁻³ | 613 |
| Rg | Radius of gyration | Å | 614 |

The raw competition data are not redistributed in this repository. Obtain the source data from the official Kaggle competition page and follow the notebook workflow.

**Source citation:** Gang Liu, Jiaxin Xu, Eric Inae, Yihan Zhu, Ying Li, Tengfei Luo, Meng Jiang, Yao Yan, Walter Reade, Sohier Dane, Addison Howard, and María Cruz. *NeurIPS - Open Polymer Prediction 2025*. Kaggle, 2025.

## Notebook workflow

| Notebook | Purpose |
|---|---|
| [`01_data_audit_and_atlas_foundation.ipynb`](notebooks/01_data_audit_and_atlas_foundation.ipynb) | Chemical integrity audit, target-overlap analysis, descriptor engineering, curated atlas foundation |
| [`02_polymer_representations_and_feature_engineering.ipynb`](notebooks/02_polymer_representations_and_feature_engineering.ipynb) | Polymer representation experiments, wildcard-retention analysis, Morgan fingerprint design, feature export |
| [`03_property_specific_baseline_modelling.ipynb`](notebooks/03_property_specific_baseline_modelling.ipynb) | Frozen random-split baselines, model-family comparison, targeted tuning, untouched holdout evaluation |
| [`04_chemistry_aware_generalization.ipynb`](notebooks/04_chemistry_aware_generalization.ipynb) | Similarity-aware reliability analysis, chemistry-separated stress tests, generalization penalties |
| [`05_shared_learning_and_reliability.ipynb`](notebooks/05_shared_learning_and_reliability.ipynb) | Masked multitask learning, complete-case ablation, ensemble disagreement, split-conformal prediction |
| [`06_interpretation_and_final_polymer_atlas.ipynb`](notebooks/06_interpretation_and_final_polymer_atlas.ipynb) | Model interpretation, Morgan motif decoding, integrated chemical-space atlas, archetypes, Pareto analysis |

## Main results

### Representation design

- All 7,973 original repeat-unit SMILES were unique and RDKit-valid.
- Removing polymer boundary information caused substantial representation collapse; the raw wildcard graph was therefore retained.
- Count Morgan radius 4 / 2048 preserved substantially more record identity than the binary fingerprint tested earlier in the workflow.
- Final representation spaces contained 36 interpretable descriptors, 2,048 Morgan count features, and a 2,084-feature Hybrid representation.

### Property-specific random-holdout performance

| Target | Final model / representation | R² | MAE | RMSE |
|---|---|---:|---:|---:|
| Tg | Extra Trees / Hybrid | 0.608 | 54.718 | 69.004 |
| FFV | HistGradientBoosting / Hybrid | 0.704 | 0.0067 | 0.0164 |
| Tc | HistGradientBoosting / Hybrid | 0.825 | 0.0249 | 0.0371 |
| Density | Ridge / Interpretable | 0.920 | 0.0275 | 0.0413 |
| Rg | HistGradientBoosting / Morgan r4 | 0.703 | 1.699 | 2.617 |

The central result is not a single best algorithm: **different polymer properties depend on different structural scales and representations**.

### Chemistry-aware generalization

Random holdout accuracy did not guarantee chemically separated performance.

- Tc showed the strongest chemistry-separated collapse: R² fell from approximately 0.825 to 0.326.
- Density remained comparatively robust, with chemistry-separated R² approximately 0.874.
- Rg showed intermediate chemical-domain sensitivity.
- FFV exhibited a strong novelty-associated typical-error penalty, although direct split comparisons were also influenced by differences in response distributions.
- Tg did not show a consistent nearest-neighbour-similarity/error relationship in the random holdout analysis.

### Shared learning

Tc, Density, and Rg formed the strongest shared measurement block, with 531 complete triplets.

Masked multitask learning used partially labelled polymers and consistently outperformed the complete-case multitask ablation. Improvements over matched single-task neural models were small and seed dependent. The principal benefit was therefore **efficient use of incomplete labels**, not a large universal positive-transfer effect.

### Reliability and uncertainty

A five-member neural ensemble plus independent split-conformal calibration was evaluated for Tc, Density, and Rg.

- Ensemble disagreement significantly ranked prediction difficulty for Tc and Density.
- Chemical support and ensemble disagreement were complementary rather than interchangeable.
- Fixed 90% split-conformal intervals achieved approximately 95.7% empirical coverage for Tc, 92.3% for Density, and 98.5% for Rg.
- Disagreement-scaled adaptive intervals were wider and did not improve the global coverage–efficiency trade-off.

A defensible reliability output is therefore treated as a combination of **point prediction, calibrated interval, model disagreement where informative, and chemical-domain support**.

### Final Polymer Atlas and chemical interpretation

Notebook 06 integrates standardized global descriptors with a 64-component Morgan latent representation into an unsupervised chemical-space visualization. The Morgan latent block retained approximately 85.7% of its variance.

Key interpretation results include:

- Tg is strongly associated with ring density and aromaticity.
- FFV depends on oxygen/carbonyl chemistry while retaining strong dependence on local Morgan environments.
- Tc is dominated by specific local/topological environments and is highly chemistry-domain sensitive.
- Density is the clearest globally interpretable/composition-driven property.
- Rg is best represented by local/topological Morgan structure.

Twenty-eight important Morgan indices were decoded back to atom-centred environments. The decoding recovered 261,846 selected-bit occurrences with zero fingerprint-count/environment-count mismatches. Sixteen selected bits showed low hashing ambiguity, eleven moderate ambiguity, and one high ambiguity.

An illustrative multi-objective scenario — maximizing Tc while minimizing Density — produced a seven-polymer Pareto front. It is reported as an application-specific design hypothesis rather than a universal ranking of polymer quality.

## Release-facing results

Curated numerical summaries are available in [`results/tables/`](results/tables/) and compact summary figures in [`results/figures/`](results/figures/). The executed notebooks retain the complete high-density diagnostic and Atlas figure set.

Key files include:

- [`final_holdout_performance.csv`](results/tables/final_holdout_performance.csv)
- [`random_vs_chemistry_separated_performance.csv`](results/tables/random_vs_chemistry_separated_performance.csv)
- [`integrated_reliability_regimes.csv`](results/tables/integrated_reliability_regimes.csv)
- [`final_property_scientific_synthesis.csv`](results/tables/final_property_scientific_synthesis.csv)
- [`final_holdout_r2.svg`](results/figures/final_holdout_r2.svg)
- [`random_vs_chemistry_r2.svg`](results/figures/random_vs_chemistry_r2.svg)
- [`hybrid_block_importance.svg`](results/figures/hybrid_block_importance.svg)
- [`conformal_coverage.svg`](results/figures/conformal_coverage.svg)

## Reproducibility

The notebooks are designed to be executed sequentially, with frozen artifacts passed from one stage to the next. Large generated arrays and model objects are intentionally not committed.

Core reproducibility principles include:

- fixed random states and exported split indices;
- target-specific observed-label subsets preserved;
- scaling and dimensionality reduction fitted only on appropriate training data when used predictively;
- final random holdouts opened only after development decisions were frozen;
- chemistry-aware validation separated from conventional random-split interpolation;
- exact Morgan fingerprint-regeneration and environment-count audits before feature interpretation;
- conformal calibration performed on an independent calibration partition.

See [`REPRODUCIBILITY.md`](REPRODUCIBILITY.md) for the execution and artifact policy.

## Repository structure

```text
polymer-informatics-atlas/
├── README.md
├── CHANGELOG.md
├── RELEASE_NOTES_v1.0.0.md
├── REPRODUCIBILITY.md
├── LICENSE
├── CITATION.cff
├── requirements.txt
├── .gitignore
├── data/
│   └── README.md
├── notebooks/
│   ├── README.md
│   └── 01_...ipynb through 06_...ipynb
├── results/
│   ├── README.md
│   ├── figures/
│   └── tables/
└── docs/
    └── scientific-defense/
```

## Software

Core Python packages include NumPy, pandas, SciPy, scikit-learn, RDKit, PyTorch, UMAP-learn, matplotlib, statsmodels, and joblib. See [`requirements.txt`](requirements.txt) for the environment-oriented dependency list.

## Scientific claim boundary

The Atlas identifies statistical structure–property relationships, predictive dependencies, generalization limits, and reliability patterns in the available dataset. It does **not** establish causal polymer-design laws. Repeat-unit SMILES do not encode every experimentally relevant variable, including molecular-weight distribution, processing history, crystallinity, morphology, or measurement conditions. Random-holdout performance should therefore not be interpreted as unrestricted extrapolation performance.

## Author

**DENNIS OBINNA ORJI**

Research portfolio in industrial chemistry, polymer science, and materials informatics.

## License

Code and original repository documentation are released under the MIT License. Source-data licensing and attribution remain governed by the original data source.
