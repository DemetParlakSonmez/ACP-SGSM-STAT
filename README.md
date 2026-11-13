# ACP-SGSM-STAT

This repository contains reproducible machine-learning workflows for
anticancer peptide (ACP) classification. The notebooks use statistical
feature selection and sequence-derived feature sets with fixed random seeds,
portable relative paths, and English documentation.

## Repository structure

| Dataset | SGSM notebook | STAT notebook | Main model |
|---|---|---|---|
| ACP-740 | [`acp740/acp_SGSM_acp740.ipynb`](./acp740/acp_SGSM_acp740.ipynb) | [`acp740/acp_STAT_acp740.ipynb`](./acp740/acp_STAT_acp740.ipynb) | SVM / soft-voting ensemble |
| AntiCP2 | [`anticp2/acp_SGSM_anticp2.ipynb`](./anticp2/acp_SGSM_anticp2.ipynb) | [`anticp2/acp_STAT_anticp2.ipynb`](./anticp2/acp_STAT_anticp2.ipynb) | Gradient boosting / CatBoost |
| DeepGram | [`deepgram/acp_SGSM_deepgram.ipynb`](./deepgram/acp_SGSM_deepgram.ipynb) | [`deepgram/acp_STAT_deepgram.ipynb`](./deepgram/acp_STAT_deepgram.ipynb) | Random forest / SVM |

Each dataset directory contains the notebook-specific CSV inputs. Feature
files in the `anticp2` and `deepgram` directories use dataset-specific
prefixes to prevent filename collisions.

## Installation

Use Python 3.10 or newer and install the dependencies from
[`requirements.txt`](./requirements.txt):

```bash
python -m venv .venv
```

Activate the environment:

```bash
# Windows PowerShell
.venv\Scripts\Activate.ps1

# macOS/Linux
source .venv/bin/activate
```

Install the required packages:

```bash
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

Then open the required notebook in Jupyter Notebook, JupyterLab, or VS Code
and run the cells from top to bottom.

## Workflow

The notebooks follow the same general pipeline:

1. Load the dataset and feature tables using relative paths.
2. Standardize numeric features with `StandardScaler`.
3. Perform Welch's two-sample t-test between the two labels.
4. Correct for multiple testing with the Benjamini-Hochberg FDR procedure.
5. Keep statistically significant features using `q < 0.01`.
6. Prepare the train-validation peptide split and remove duplicate or
   conflicting peptide entries.
7. Train the dataset-specific classifier.
8. Report cross-validation and validation metrics, including accuracy, F1,
   sensitivity, specificity, precision, ROC-AUC, and MCC where applicable.

The notebooks preserve the original model parameters and random state values
used for the analyses. A warning about precision loss may appear during the
t-test for nearly identical feature values; this is a data characteristic,
not an installation failure.

## Data and paths

Run a notebook from the repository root or from its dataset directory. The
notebooks search the supported relative locations for their input files and
do not depend on local Windows-specific absolute paths.

The repository includes the following dataset-specific inputs:

- `acp740/`: ACP-740 combined and feature CSV files.
- `anticp2/`: AntiCP2 combined, split, and `anticp2_features_*.csv` files.
- `deepgram/`: DeepGram combined, split, and `deepgram_features_*.csv` files.

## Reproducibility

The workflows set deterministic seeds where supported by the estimator and
use fixed cross-validation configuration. Exact results can still vary
slightly with changes in Python, scikit-learn, SciPy, CatBoost, or numerical
library versions.

## License and data note

This repository is intended for research and educational use. Confirm the
license and redistribution conditions of the underlying datasets and feature
resources before using them in another project.
