# Proposal

## Why

The project has a synthetic lead dataset but no notebook that looks at it: nobody has yet seen the distributions of age, income, loan amount, employment and loan purpose, or the conversion rate across them. The company's real data is not yet available. When it arrives, the assumptions behind the synthetic data (units, conversion rate, purpose distribution) must be compared with it, and today there is no tool for that comparison.

## What Changes

- Add `notebooks/02_eda.ipynb`, an exploratory data analysis of one lead dataset in the data dictionary's schema: overview and missing values, applicant profile, employment, income and existing credit, the requested loan, lead timing, and conversion rate by segment.
- The notebook is parameterized: `DATA_PATH` is the dataset to explore (default `data/synthetic_leads.csv`), and an optional `COMPARE_PATH` (default `None`) is a second dataset. With `COMPARE_PATH` unset, the notebook explores one dataset. With it set, every plot shows both datasets and a comparison section scores each column with a verdict (similar, moderate shift, shifted), using the KS statistic for numeric columns and total variation distance for categorical columns.
- Verdict thresholds, and the dataset labels used in plots, are also parameters.
- Both datasets are validated against the data dictionary before analysis; the notebook shows aggregates only, never individual rows.
- Add the notebook to the `notebooks/README.md` index.

## Capabilities

### New Capabilities
- `eda-notebook`: the exploratory analysis notebook and its comparison mode: parameters, schema validation, the topics the exploration covers, the comparison statistics and verdicts, aggregate-only output, and the statement that synthetic results say nothing about real-data performance.

### Modified Capabilities

None. `synthetic-data-generator` already requires that analysis notebooks read only a dictionary-schema CSV from `data/`; this change implements that contract and does not alter it. `project-layout` already requires numbered notebooks listed in `notebooks/README.md`.

## Impact

- New: `notebooks/02_eda.ipynb`, `openspec/specs/eda-notebook/spec.md` (on archive).
- Edited: `notebooks/README.md` (index row); `docs/data/eda-notes.md` receives the findings, linked to the notebook.
- Dependencies: pandas, numpy, matplotlib and scipy (for `ks_2samp`); check which are already in the project environment.
- Out of scope: feature engineering and saved derived features, model training, tuning the verdict thresholds against real data (done once production data exists), and any change to the data generator.
