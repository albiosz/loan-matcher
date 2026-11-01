# Tasks

## 1. Setup and loading

- [x] 1.1 Install matplotlib and scipy into `.venv` and verify `python -c "import matplotlib, scipy.stats"` succeeds; note the two packages in `notebooks/README.md` (no environment file exists yet) and verify the note is present
- [x] 1.2 Create `notebooks/02_eda.ipynb` with the goal cell, the `parameters`-tagged cell (`DATA_PATH`, `COMPARE_PATH`, dataset labels, verdict thresholds), and the loader that reads the dictionary's field list; verify it runs top to bottom on the defaults and loads 50,000 rows with 25 columns
- [x] 1.3 Add the explicit numeric / categorical / flag column lists and the schema check (dictionary versus dataset columns, and lists versus dictionary); verify a CSV with one column dropped stops the notebook with a message naming that column, and the defaults pass

## 2. Single-dataset exploration

- [x] 2.1 Add the overview (shape, types, missing values per column, no raw rows) and the applicant profile section (age, applicants, children, living arrangements, occupation, education), with plot helpers that take a dict of label -> DataFrame; verify the section renders for the default dataset and shows no raw rows
- [x] 2.2 Add the employment and residence section (years conversion stated, not employed shown separately, probation, mini job, temporary job) and the income and existing credit section (clipped display range, scores unaffected); verify the sections run and tenure is displayed in years
- [x] 2.3 Add the loan section (amount, duration, purpose shares) and the lead timing section (hour, weekday, month, one panel per dataset); verify the purpose shares sum to 100% and timing shows the daytime concentration
- [x] 2.4 Add the target section: overall conversion, conversion by purpose, age group, disposable income group and employment flags, plus the affordability view (payment divided by duration, ratio to disposable income, no saved columns); verify the overall rate is near 3% and that no file in `data/` changes

## 3. Comparison mode

- [x] 3.1 Implement the column scores (KS for numeric, total variation for categorical and flags, non-missing values only, ids and time excluded, "not comparable" when a column is empty in one dataset) and the verdicts with the sorted table and summary line; verify with the file compared to itself (all scores 0, all "similar")
- [x] 3.2 Add the comparison-only cells: overlays and side-by-side shares in the plots from group 2 (shares or densities, no counts), conversion rate and missingness side by side with absolute and relative difference, and the note that a verdict flags a difference to inspect; verify they are skipped when `COMPARE_PATH` is none and appear when it is set
- [x] 3.3 Verify comparison mode against three cases: income multiplied by 12 in memory (income score near 1, verdict "shifted"), a regeneration of the synthetic data with another seed written to a git-ignored file in `data/` from a temporary copy of `01_synthetic_data.ipynb` (every verdict "similar"), and changed threshold parameters (verdicts follow); record the observed scores for the same-generator case in the design notes if any column exceeds 0.10

## 4. Wrap-up

- [x] 4.1 Write the introduction and conclusion cells (goal, units, synthetic results are generator assumptions and say nothing about real leads, aggregate-only rule, warning about exporting production comparison runs); verify against the "Findings and limitations are stated" requirement
- [x] 4.2 Record the main findings in `docs/data/eda-notes.md` with a link to the notebook, labeled as properties of the synthetic data, and add the `02_eda.ipynb` row to the `notebooks/README.md` index; verify both files and that the notebook runs from a clean kernel (restart and run all) without errors
- [x] 4.3 Add a journal entry `docs/journal/YYYY-MM-DD.md` with Did, Learned and Next sections, including how to use `COMPARE_PATH` once the production file arrives; verify the three sections are present
- [x] 4.4 Stage the notebook and confirm the committed version has no outputs or execution counts (`git diff --cached notebooks/02_eda.ipynb` shows `"outputs": []`); do not commit unless asked

## Workflow follow-up

- Archive the change after review so `eda-notebook` becomes a main spec.
- When production data arrives, run the notebook with `COMPARE_PATH` set, then revisit the thresholds and the generator assumptions the verdicts flag.
