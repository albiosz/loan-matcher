# Proposal

## Why

The real company data is not yet accessible, so the project runs on synthetic leads produced by `notebooks/01_synthetic_data.ipynb`. The generator encodes many assumptions (conversion rate, effect of income and loan purpose, noise, missing values) that are written only in notebook code and comments. Without a specification, later notebooks and the final report cannot state what the synthetic results do and do not mean, and the generator cannot be adjusted or replaced by real data in a controlled way.

## What Changes

- Add a specification for the synthetic data generator: its contract with the data dictionary, the data it produces, how the target is generated, the assumptions that are intentionally unvalidated, and how real data takes its place.
- Document the generator's current behavior as requirements; no change to generator behavior is intended.
- Record the business inputs the assumptions rest on (about 3% conversion, loan takers tend to be people who need money, nearly any lead can convert) so they can be corrected once real data is seen.

## Capabilities

### New Capabilities
- `synthetic-data-generator`: schema contract with the data dictionary, determinism, field-level generation rules, the conversion model (eligibility, intent, noise, calibrated rate), missing values, output location and the rules for switching to real data.

### Modified Capabilities

None. `project-layout` already requires the data dictionary and a git-ignored `data/`; this change adds behavior on top of them without altering those requirements.

## Impact

- New: `openspec/specs/synthetic-data-generator/spec.md` (on archive).
- No code, notebook or dependency changes. Any divergence found between the spec and `notebooks/01_synthetic_data.ipynb` while implementing the tasks is fixed in the notebook or recorded as a spec correction.
- Out of scope: validating the assumptions against real data, choosing the model, and evaluation metrics for the final model.
