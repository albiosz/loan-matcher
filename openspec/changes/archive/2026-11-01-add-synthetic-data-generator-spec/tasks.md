# Tasks

## 1. Align the generator with the spec

- [x] 1.1 In `notebooks/01_synthetic_data.ipynb`, give every employed applicant at least 1 month of `employment_duration` (including 18-year-olds) so `in_probation` and `mini_job` never occur with a tenure of 0; verify with a check that no row has `in_probation = 1` or `mini_job = 1` together with `employment_duration = 0`
- [x] 1.2 Add assertions to the notebook for the deterministic scenarios (disposable income identity, `relationship_between_applicants` only with two applicants, retired implies age 63 or older, ids and `prediction_at` increasing together, no missing values in identifiers, `prediction_at` and the target); verify the notebook executes without assertion errors
- [x] 1.3 Verify reproducibility: run the generator twice with the same seed and confirm identical output (for example with a hash of the CSV), and add this check to the notebook

## 2. Verify the statistical scenarios

- [x] 2.1 Run the notebook with default settings and confirm the conversion rate is within 0.3 percentage points of 3%, and with two other seeds that it stays in that range
- [x] 2.2 Confirm from the notebook's sanity output that unaffordable leads convert far less than average, `dispo` and `debt_conversion` convert more than `journey` and `other`, probation and mini-job leads convert less than others, and high-income leads still convert; record any scenario that fails and fix the generator or correct the spec
- [x] 2.3 Confirm the baseline model's ROC AUC on the time split is clearly below 1 and below the noise-free signal's AUC

## 3. Documentation

- [x] 3.1 Make the notebook's introduction and conclusion state the units, the field semantics (`main_income`, `other_income`, `disposable_income`, `employment_duration = 0`) and that synthetic scores are a pipeline check only; verify by reading both cells against the "Units and field semantics" and "pipeline check" requirements
- [x] 3.2 Update `data/README.md` and `docs/data/data-dictionary.md` if needed so the generator, the real-data placeholder and the loan purposes list stay consistent; verify the dictionary still parses (25 fields, 10 purposes) by running the notebook
- [x] 3.3 Add a journal entry `docs/journal/YYYY-MM-DD.md` (Did, Learned, Next) summarizing the generator assumptions and the business inputs behind them; verify it follows the sections required by `project-layout`

## 4. Integration check

- [x] 4.1 Run `openspec validate add-synthetic-data-generator-spec --strict` and verify it passes, then walk through every scenario in the spec once against the final CSV and notebook output

## Workflow follow-up

- Archive the change once the checks above pass, so the spec becomes `openspec/specs/synthetic-data-generator/spec.md`.
- Revisit the spec when real data becomes available (conversion rate, units, purpose distribution).
