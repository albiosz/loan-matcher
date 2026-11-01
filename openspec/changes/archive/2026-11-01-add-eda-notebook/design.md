# Design

## Context

`data/synthetic_leads.csv` (50,000 rows, 25 columns) exists and follows the data dictionary. The dictionary lists every field and its group but its Type and Meaning columns are still empty, so the notebook cannot read column types from it. There is no `src/` folder and no dependency file; the `.venv` has pandas and numpy but not matplotlib or scipy. Notebook outputs are stripped on commit (nbstripout), so results live only in the local run. See proposal.md for motivation and the spec for requirements.

## Goals / Non-Goals

**Goals:**
- One notebook that serves as plain EDA today and as the synthetic-versus-production comparison later, with a single parameter cell as the only thing to edit.
- A comparison whose verdicts a reader can understand without statistics background.

**Non-Goals:**
- Statistical hypothesis testing (no p-values), feature engineering, or modeling.
- Importable helper modules; the project keeps logic inside notebooks.

## Decisions

- **One notebook, parameter cell at the top.** `DATA_PATH`, `COMPARE_PATH`, labels and thresholds sit in one cell tagged `parameters`, so the notebook can be driven by papermill later without adding it now. Alternative: separate EDA and comparison notebooks; rejected because the comparison reuses every plot and the two would drift.
- **Plot code handles one or two datasets uniformly.** Datasets are held in a dict label -> DataFrame (one entry or two), and each plot helper loops over it. Single-dataset mode is the same code with one entry, so comparison mode is exercised by every plot rather than being dormant code.
- **Explicit column groups in the notebook.** Numeric, categorical and 0/1 flag columns are listed in a cell, and a check fails if the lists do not cover exactly the dictionary's fields (minus ids and time). 0/1 flags use the categorical score, where total variation equals the difference in shares. Alternative: infer types from dtypes; rejected because discrete codes and flags would be guessed wrong and a new field would be silently mis-typed.
- **KS and total variation distance.** Both are 0 to 1, need no binning, and do not depend on dataset size. Scores use non-missing values; missingness is compared separately. A column with no non-missing values in one dataset gets the verdict "not comparable". Alternative: PSI, the credit-scoring convention; deferred because it needs binning choices and is harder to explain. Computing KS with `scipy.stats.ks_2samp` is chosen over a hand-written version for correctness.
- **Verdict thresholds 0.10 and 0.20 as parameters.** Two draws of the same distribution at this sample size score near 0.01, so 0.10 is well above noise, and a unit mismatch (for example EUR per year versus per month) scores near 1. They are rules of thumb to be revisited on real data.
- **Shared plotting ranges.** Overlays use bins computed from the pooled data and clip heavy-tailed fields (incomes, debt) to the 1st-99th percentile for display only; scores always use all values. Counts are never plotted in comparison mode.
- **Years for tenure.** Employment and residence durations are converted from months to years for display only, with the conversion stated in the notebook. Zero employment duration (not employed) is shown separately from employed tenure.
- **Aggregates only.** The notebook never calls a bare `head()` or displays rows. A schema summary and `describe()` replace it. Categories with very few rows are shown, since the data is already aggregated per category, but nothing is shown per application.
- **Findings go to `docs/data/eda-notes.md`.** That file already exists for findings; the notebook's conclusion links there. The notes record findings about the synthetic data labeled as generator assumptions.
- **Comparison mode is verified without a second real dataset.** Three checks, run once during implementation, not stored as test code: the file against itself (all scores 0), income scaled by 12 in memory (score near 1), and a regeneration of the synthetic data with another seed (all "similar"). The regeneration uses a temporary copy of `01_synthetic_data.ipynb` with a different seed and output path, so the numbered notebook is not edited.

## Risks / Trade-offs

- [Thresholds are unvalidated] → They are parameters, the notebook says they are defaults, and tuning against real data is listed as out of scope and an open question.
- [KS sees only the largest gap and is steppy on discrete columns or columns with a mass at zero] → The overlay plots sit next to every score, and the notebook says a verdict flags a difference to inspect.
- [Comparison output from real data could end up in git or slides] → Outputs are stripped on commit; the notebook warns that exports of comparison runs on production data go to `docs/presentations/` only after review.
- [New dependencies: matplotlib, scipy] → Installed into `.venv` as the first task, and the README or environment note is updated if the project documents its environment.
- [Notebook logic duplicates nothing but is not unit tested] → Mitigated by the three verification runs above and by clean-kernel execution.

## Open Questions

- Should the thresholds differ by field kind (for example looser for rare categories) once real data shows how noisy small categories are? Answerable after the first real comparison without changing the specs.
