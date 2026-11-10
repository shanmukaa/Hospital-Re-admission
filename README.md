# Hospital Readmission Risk Analysis

An exploratory data analysis project using the Diabetes 130-US Hospitals dataset to investigate hospital readmission patterns for patients with diabetes. The analysis examines encounter, diagnosis, medication, admission, and discharge data and prepares the dataset for a future model that could help prioritize post-discharge follow-up.

> **Scope:** The notebook documents data exploration and preprocessing decisions. Its problem canvas describes an MLP-based classifier and evaluation metrics as project goals; do not treat these goals as validated model results or clinical guidance.

## Repository contents

| File | Description |
| --- | --- |
| `Hospital Readmission.ipynb` | Jupyter notebook for data inspection, exploratory analysis, missing-value handling, and preparation of eligible encounters. |
| `diabetic_data.csv` | Encounter-level data from the Diabetes 130-US Hospitals dataset. The notebook reports 101,766 records and 50 columns in the supplied CSV. |
| `IDS_mapping.csv` | Descriptions for admission type, discharge disposition, and admission source IDs used in the encounter data. |
| `Shanmukha_HOSPITAL READMISSION RISK .docx` | Supporting project report. |

## Dataset and analysis

The data represents hospital encounters from 1999–2008. The notebook explores readmission status (`<30`, `>30`, and `NO`), demographics, diagnoses, hospital utilization, and diabetes medication information. It also examines missingness, unique values, encounter-level patterns, and the relationship between discharge disposition and readmission.

Preprocessing documented in the notebook includes:

- Excluding encounters with discharge disposition IDs `11`, `19`, and `20`, which represent expired patients and are not eligible for a subsequent readmission in this analysis.
- Converting the dataset's `?` markers into missing values.
- Dropping `weight`, which has very high missingness, and representing unmeasured glucose and A1C results as `Not_Measured`.
- Representing missing categorical values such as race, payer, specialty, and diagnosis codes as `Unknown` rather than guessing a category.
- Removing constant medication columns (`examide` and `citoglipton`) and inspecting numerical features for outliers.

The notebook's eligible-encounter analysis reports 100,114 encounters after the expired-patient exclusion. These are encounters, not unique patients; the notebook notes that some patients have multiple encounters, which should be considered when designing any train/test split.

## Requirements

The notebook imports Python packages `pandas`, `numpy`, `matplotlib`, and `seaborn`. You will also need Jupyter Notebook or JupyterLab to run it.

```bash
python -m venv .venv
# Activate the environment, then install the packages:
python -m pip install pandas numpy matplotlib seaborn jupyter
jupyter notebook
```

Open `Hospital Readmission.ipynb` in Jupyter and run the cells in order.

## Data paths

The notebook's initial data-loading cell currently contains absolute Windows paths from the original author's computer. To run it in a fresh clone, change those two paths to repository-relative paths:

```python
df = pd.read_csv("diabetic_data.csv")
ids_mapping = pd.read_csv("IDS_mapping.csv")
```

Keep both CSV files in the repository root, or update the paths to wherever you store them locally.

## Readmission target and intended next steps

The source target has three categories: `<30` (readmitted within 30 days), `>30` (readmitted after 30 days), and `NO` (no recorded readmission). The notebook's project canvas proposes a binary target for readmission within 30 days and an MLP-based classifier, assessed with accuracy, precision, recall, F1-score, and ROC-AUC. Before treating such a model as useful, further work would need to define the binary-label transformation, use patient-level data splitting to avoid leakage across repeated encounters, handle class imbalance, and report evaluation results. The repository should not be used to make clinical decisions.

## Data source

The data is commonly known as the **Diabetes 130-US Hospitals for Years 1999–2008** dataset. See the [UCI Machine Learning Repository dataset page](https://archive.ics.uci.edu/dataset/296/diabetes+130-us+hospitals+for+years+1999-2008) for source and dataset details.
