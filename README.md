# VASC-Guard

VASC-Guard is a research notebook for retinal vessel segmentation and selective post-processing. It evaluates whether proposed vessel repairs improve a frozen segmentation while limiting pixel-level and structural harm.

The workflow is designed for Kaggle and evaluates a selective VASC-Guard model on DRIVE, STARE, and CHASE_DB1.

## What the notebook does

- Loads frozen VASC-Guard checkpoints and validation-only thresholds.
- Runs full-resolution, sliding-window retinal vessel inference.
- Builds deterministic repair candidates, including endpoint bridges and removal of small isolated components.
- Learns correction-yield and structural-risk estimators from DRIVE development data only.
- Compares multiple repair policies at matched intervention budgets.
- Reports overlap, connectivity, calibration, harm, and abstention metrics.
- Exports audit records, candidate tables, trained estimators, reports, and publication-ready figures.

## Repository contents

```text
.
├── VASC_Guard_Manuscript_Complete_Kaggle.ipynb
└── README.md
```

## Technologies used

- Python 3
- PyTorch
- NumPy and pandas
- OpenCV and Pillow
- SciPy and scikit-image
- scikit-learn
- Matplotlib
- Hugging Face Hub
- Jupyter Notebook / Kaggle Notebooks

## Data and required artifacts

This study uses the DRIVE, STARE, and CHASE_DB1 retinal vessel datasets. They are available from their respective providers, subject to each provider's access and usage conditions:

- [DRIVE](https://drive.grand-challenge.org/)
- [STARE](https://cecas.clemson.edu/~ahoover/stare/)
- [CHASE_DB1](https://researchinnovation.kingston.ac.uk/en/datasets/chasedb1-retinal-vessel-reference-dataset-4/)

For reproducible notebook execution, the code expects the following mounted resources:

| Resource | Expected runtime source |
| --- | --- |
| DRIVE | `Zomba/DRIVE-digital-retinal-images-for-vessel-extraction` on Hugging Face |
| CHASE_DB1 | Kaggle dataset mounted at `/kaggle/input/datasets/khoongweihao/chasedb1` |
| STARE | Kaggle dataset mounted at `/kaggle/input/datasets/jitushimul/stare-data/STARE` |
| Frozen VASC-Guard artifacts | Kaggle dataset mounted at `/kaggle/input/datasets/jitushimul/vasguard/VASC_Guard_RUN_001` |

The frozen artifact bundle must contain the expected model checkpoints, `locked_manifest.csv`, and `model_selection_VALIDATION_ONLY.csv`.

## How to use it

1. Create a Kaggle notebook and upload `VASC_Guard_Manuscript_Complete_Kaggle.ipynb`.
2. Attach the VASC-Guard artifact bundle, CHASE_DB1, and STARE datasets listed above.
3. Enable Internet access if DRIVE is not already available in the notebook session. The notebook can download the specified DRIVE mirror from Hugging Face.
4. Select a GPU accelerator if available. CPU execution is supported but will be slower.
5. Review the `CFG` dataclass in the first cell and update mount paths only if your Kaggle inputs use different locations.
6. Run all cells in order.

The default output directory is:

```text
/kaggle/working/VASC_Guard_RiskBenefit_Stage5_Structural_Validated
```

## Main outputs

The notebook creates the following folders under the output directory:

- `audit/` — manifests, checkpoint records, configuration, and protocol details.
- `candidate_tables/` — generated and scored repair candidates.
- `metrics/` — per-image, cross-validation, and budget-matched evaluation results.
- `figures/` — plots, six-panel comparisons, and zoomed visualizations.
- `maps/` — optional prediction maps when enabled in `CFG`.
- `models/` — fitted development estimators.
- `reports/` — summary tables and manuscript-oriented results.

## Reproducibility notes

- Default seeds are `42`, `123`, and `2026`.
- External ground truth is used for final evaluation, not repair-policy selection.
- Candidate generation uses prediction, morphology, risk, and gating evidence without evaluation ground truth.
- The notebook validates dataset layouts, image/mask dimensions, checkpoint identities, and key metric invariants before producing results.

## Important limitation

This repository contains research code, not a clinical decision-support system. Results are exploratory and must not be interpreted as medical advice or as evidence of clinical safety or effectiveness.
