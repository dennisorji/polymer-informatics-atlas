# Results

This directory contains lightweight, release-facing outputs from the executed Polymer Informatics Atlas workflow. Large intermediate arrays, model checkpoints, fitted transformation objects, and notebook export archives are intentionally excluded from Git history.

## Figures

Curated summary figures are in [`figures/`](figures/):

- [`final_holdout_r2.svg`](figures/final_holdout_r2.svg) — final untouched random-holdout R² by property;
- [`random_vs_chemistry_r2.svg`](figures/random_vs_chemistry_r2.svg) — random versus chemistry-separated performance;
- [`hybrid_block_importance.svg`](figures/hybrid_block_importance.svg) — dependence of Hybrid models on interpretable and Morgan blocks;
- [`conformal_coverage.svg`](figures/conformal_coverage.svg) — fixed split-conformal empirical coverage;
- [`complete_block_pearson_correlation.svg`](figures/complete_block_pearson_correlation.svg) — Tc–Density–Rg complete-case correlation structure.

The executed notebooks contain the full high-density figure set, including the integrated UMAP Polymer Atlas, five experimental-property landscapes, holdout-error landscapes, chemical-domain support maps, ensemble-uncertainty maps, decoded Morgan-environment drawings, property archetypes, and the illustrative Tc–Density Pareto front.

## Tables

Release-facing numerical summaries are in [`tables/`](tables/), including final holdout metrics, chemistry-separated stress-test results, representation-block importance, conformal coverage, validated Morgan-feature summaries, reliability regimes, complete-block correlations, and the final property-specific scientific synthesis.

Values in this directory are summaries of the executed notebooks. Fold-level, per-polymer, bootstrap, residual, and environment-occurrence outputs remain in the notebooks or their excluded local export archives.
