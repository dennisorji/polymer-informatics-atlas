# Data

The raw NeurIPS Open Polymer Prediction 2025 / Kaggle dataset is not redistributed in this repository.

The project uses the official `train.csv` as its core analysis table. After integrity auditing, the working population contains 7,973 unique, RDKit-valid polymer repeat-unit records with sparse measurements for:

- Tg — glass-transition temperature (°C): 511 records;
- FFV — fractional free volume: 7,030 records;
- Tc — thermal conductivity (W m⁻¹ K⁻¹): 737 records;
- Density — polymer density (g cm⁻³): 613 records;
- Rg — radius of gyration (Å): 614 records.

Official source: https://www.kaggle.com/competitions/neurips-open-polymer-prediction-2025/data

Recommended source citation:

> Gang Liu, Jiaxin Xu, Eric Inae, Yihan Zhu, Ying Li, Tengfei Luo, Meng Jiang, Yao Yan, Walter Reade, Sohier Dane, Addison Howard, and María Cruz. *NeurIPS - Open Polymer Prediction 2025*. Kaggle, 2025.

To reproduce the workflow, obtain the source data from the official competition page and run the notebooks in order. Generated intermediate arrays, fitted model objects, neural-network weights, and notebook ZIP exports are intentionally excluded from version control because they are reproducible artifacts and can be large.

The repository's MIT License applies to original code and documentation in this repository. Source-data licensing and attribution remain governed by the original dataset distribution.
