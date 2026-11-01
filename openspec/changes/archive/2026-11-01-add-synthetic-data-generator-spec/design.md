# Design

## Context

The generator already exists as `notebooks/01_synthetic_data.ipynb` and writes `data/synthetic_leads.csv`. It reads the field list and the loan purposes from `docs/data/data-dictionary.md`, so the dictionary is the schema source of truth. The repository has no `src/` folder, and adding a top-level folder would require a `project-layout` change, so the generator stays a notebook. See proposal.md for motivation.

Checking the current output against the draft spec found one divergence: applicants aged 18 get a tenure capped at 0 months, so 609 of 50,000 rows are flagged `in_probation` while `employment_duration` is 0 (which otherwise means "not employed").

## Goals / Non-Goals

**Goals:**
- Make the generator's behavior and assumptions reviewable without reading code.
- Keep the spec at the level of observable output so the notebook can be refactored.

**Non-Goals:**
- Changing conversion mechanics or parameters beyond fixing divergences from the spec.
- Matching real-data distributions; they are unknown.
- Moving the generator out of `notebooks/` or adding dependencies.

## Decisions

- **One capability, `synthetic-data-generator`.** Schema, field rules, outcome model and switch-over all describe the same artifact and change together. Alternative: split into `synthetic-schema` and `synthetic-outcome`; rejected as two specs for one 250-line notebook.
- **Behavior in the spec, numbers in the notebook.** The spec fixes the direction of effects (need raises intent, unaffordability suppresses conversion, acute-need purposes convert more) and the default 3% rate. Exact coefficients, shares and typical amounts stay in the notebook's configuration, because they are guesses expected to change. Alternative: list every coefficient in the spec; rejected because every tuning would become a spec change.
- **Scenario tolerances are loose.** Rate within 0.3 percentage points and "at least as often" comparisons survive seed changes; exact equalities are limited to deterministic identities (disposable income, ids vs time).
- **Dictionary-driven checks stay.** The schema and purpose-list assertions are part of the contract, so adding a field or purpose to the dictionary forces a generator update.
- **Fix the 18-year-old divergence in the generator, not by weakening the spec.** An employed applicant gets at least 1 month of tenure. The alternative of allowing probation with zero tenure would make `employment_duration = 0` ambiguous for the future real-data model.

## Risks / Trade-offs

- [Spec describes guesses as requirements] → Purpose and the last requirement state that results are a pipeline check, and the assumptions are listed for review once real data arrives.
- [Spec drifts from the notebook] → Tasks include verifying every scenario against the generated CSV; later notebook edits that change behavior go through `/opsx:*`.
- [Spec checks only hold for the default size] → Scenarios refer to default settings or to relative comparisons; very small row counts are not covered.
