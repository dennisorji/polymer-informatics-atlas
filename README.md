# Polymer Informatics Atlas

A chemistry-aware polymer informatics study integrating structure–property modelling, chemical-domain generalization, multitask learning, uncertainty quantification, and interpretable polymer design.

## Overview

The **Polymer Informatics Atlas** is a six-notebook research workflow built around 7,973 polymer repeat-unit records from the NeurIPS Open Polymer Prediction 2025 dataset. The project moves from chemical data auditing and representation design to property-specific modelling, chemistry-aware stress testing, multitask learning, calibrated reliability, and an integrated structure–property atlas.

The project is designed as a research study rather than a leaderboard exercise. Every modelling stage is separated from the next so that representation choices, interpolation performance, chemical-domain generalization, transfer learning, and uncertainty can be defended independently.

## Research questions

- Which polymer representations preserve chemically meaningful repeat-unit information?
- Which properties are captured by global interpretable descriptors versus local/topological Morgan environments?
- How much does random-split performance overestimate chemically separated generalization?
- Can partially labelled polymer data improve shared learning?
- Can prediction-level uncertainty and chemical-domain support identify unreliable predictions?
- Which structural motifs and cross-property trade-offs emerge from the final models?

## Dataset

Core dataset: **7,973 unique, RDKit-valid polymer repeat units**.

Observed target counts:

| Property | Records |
|---|---:|
| Tg | 511 |
| FFV | 7,030 |
| Tc | 737 |
| Density | 613 |
| Rg | 614 |

The raw competition data are **not redistributed in this repository**. Users should obtain the source data from the original NeurIPS Open Polymer Prediction 2025 / Kaggle distribution and follow the notebook workflow.

## Notebook workflow

| Notebook | Purpose |
|---|---|
| `01_data_audit_and_atlas_foundation.ipynb` | Chemical integrity audit, target-overlap analysis, descriptor engineering, curated atlas foundation |
| `02_polymer_representations_and_feature_engineering.ipynb` | Polymer representation experiments, wildcard-retention analysis, Morgan fingerprint design, feature export |
| `03_property_specific_baseline_modelling.ipynb` | Frozen random-split baselines, model-family comparison, targeted tuning, untouched holdout evaluation |
| `04_chemistry_aware_generalization.ipynb` | Similarity-aware reliability analysis, chemistry-separated stress tests, generalization penalties |
| `05_shared_learning_and_reliability.ipynb` | Masked multitask learning, complete-case ablation, ensemble disagreement, split-conformal prediction |
| `06_interpretation_and_final_polymer_atlas.ipynb` | Model interpretation, Morgan motif decoding, integrated chemical-space atlas, archetypes, Pareto analysis |

## Main results

### Representation design

- All 7,973 original repeat-unit SMILES were unique and RDKit-valid.
- Removing polymer boundary information caused substantial representation collapse; the raw wildcard graph was therefore retained.
- Count Morgan radius 4 / 2048 preserved substantially more identity than binary fingerprints.
- Final representation spaces were 36 interpretable descriptors, 2,048 Morgan count features, and a 2,084-feature hybrid.

### Property-specific random-holdout performance

Final Notebook 03 holdout R² values:

| Target | Final model / representation | R² |
|---|---|---:|
| Tg | Extra Trees / Hybrid | 0.608 |
| FFV | HistGradientBoosting / Hybrid | 0.704 |
| Tc | HistGradientBoosting / Hybrid | 0.825 |
| Density | Ridge / Interpretable | 0.920 |
| Rg | HistGradientBoosting / Morgan r4 | 0.703 |

The key result is not a single best algorithm: **different polymer properties depend on different structural scales and representations**.

### Chemistry-aware generalization

Random holdout accuracy did not guarantee chemically separated performance.

- Tc showed the strongest chemistry-separated collapse: R² fell from about 0.825 to about 0.326.
- Density remained comparatively robust.
- Rg showed intermediate chemical-domain sensitivity.
- FFV exhibited a strong novelty-associated error penalty, although response-distribution differences also mattered.

### Shared learning

Tc, Density, and Rg formed the strongest shared measurement block.

Masked multitask learning outperformed complete-case multitask learning by exploiting partially labelled polymers, but improvements over matched single-task neural networks were small and seed dependent. The main benefit was therefore **data efficiency**, not universal positive transfer.

### Reliability and uncertainty

A five-member neural ensemble plus independent split-conformal calibration was used for Tc, Density, and Rg.

- Ensemble disagreement significantly ranked prediction difficulty for Tc and Density.
- Chemical support and ensemble disagreement were complementary rather than interchangeable.
- Fixed 90% split-conformal intervals achieved approximately 95.7% coverage for Tc, 92.3% for Density, and 98.5% for Rg.
- Disagreement-scaled adaptive intervals were wider and did not improve the global coverage–efficiency trade-off.

The resulting reliability principle is:

> **point prediction + calibrated interval + model disagreement + chemical-domain support**

### Final Polymer Atlas

The final notebook integrates global descriptors and compressed Morgan structure into an unsupervised 2D chemical-space atlas.

Key interpretation results include:

- Tg is strongly associated with ring density and aromaticity.
- FFV depends on oxygen/carbonyl chemistry but also strongly on local Morgan environments.
- Tc is dominated by specific local/topological environments and is highly chemistry-domain sensitive.
- Density is the clearest globally interpretable/composition-driven property.
- Rg is best represented by local/topological Morgan structure.

Twenty-eight important Morgan indices were decoded back to atom-centred environments. Most showed low or moderate hashing ambiguity, enabling chemically cautious motif interpretation.

An illustrative multi-objective scenario — maximizing Tc while minimizing Density — produced a seven-polymer Pareto front and was treated explicitly as an application-specific design hypothesis rather than a universal ranking.

## Repository structure

```text
polymer-informatics-atlas/
├── README.md
├── LICENSE
├── CITATION.cff
├── requirements.txt
├── .gitignore
├── data/
│   └── README.md
├── notebooks/
│   └── README.md
├── results/
│   └── README.md
└── docs/
    └── scientific-defense/
```

## Reproducibility principles

- Fixed random states and exported split indices.
- Target-specific observed-label subsets preserved.
- Scaling and dimensionality reduction fitted only on appropriate training data when used predictively.
- Final random holdouts opened only after development decisions were frozen.
- Chemistry-aware validation separated from conventional random-split interpolation.
- Morgan feature interpretation verified against exact regenerated fingerprints and recovered environment counts.
- Conformal calibration performed on an independent calibration partition.

## Software

Core Python packages include:

- NumPy
- pandas
- SciPy
- scikit-learn
- RDKit
- PyTorch
- UMAP-learn
- matplotlib
- statsmodels
- joblib

See `requirements.txt` for the reproducibility-oriented dependency list.

## Scientific claim boundary

The Atlas identifies statistical structure–property relationships, predictive dependencies, generalization limits, and reliability patterns in the available dataset. It does **not** establish causal polymer-design laws, and it does not encode every experimentally relevant variable such as molecular-weight distribution, processing history, crystallinity, morphology, or measurement conditions.

## Author

**DENNIS OBINNA ORJI**

Industrial Chemistry / materials-informatics research portfolio.

## License

Code and original repository documentation are released under the MIT License. Dataset licensing and redistribution remain governed by the original data source.
