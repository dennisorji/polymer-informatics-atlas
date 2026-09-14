# Notebooks

The research workflow is organized into six notebooks that should be run in order:

1. Data audit and atlas foundation
2. Polymer representations and feature engineering
3. Property-specific baseline modelling
4. Chemistry-aware generalization
5. Shared learning and reliability
6. Interpretation and final Polymer Atlas

Each notebook freezes and exports the artifacts needed by subsequent stages. Expensive modelling decisions are not revisited after holdout evaluation, and chemistry-aware validation is kept separate from conventional random-split interpolation.
