# Proposal

## Why

The loan-matcher project (a Deep Learning course project predicting whether a loan lead converts within 30 days) starts with an empty repository. Work will arrive as notebooks, text documents, presentations, papers and links. Without an agreed structure from day one, material gets scattered and the reasoning behind results is lost. The notebooks are the real deliverable, so they need a first-class home from the start.

## What Changes

- Add a top-level `notebooks/` folder as the main work area, with numbered notebooks (`01_*.ipynb`, ...) and a `notebooks/examples/` subfolder for course examples kept as read-only reference.
- Add a top-level `data/` folder for datasets, kept out of git by default (loan applications are personal data) with a documented way to obtain the data.
- Add a `docs/` folder with:
  - `project/` (brief, requirements, timeline)
  - `data/` (data dictionary, EDA notes)
  - `journal/` (dated work log, `YYYY-MM-DD.md`)
  - `decisions/` (short numbered decision records)
  - `presentations/` (slides, source and exported PDF)
  - `references/` (`links.md`, `papers/`, `course/`)
- Add a short README to each folder explaining its purpose, plus a map in `docs/README.md` and an expanded top-level `README.md`.
- Move the root `LeadConversionRecord` schema file into `docs/data/data-dictionary.md`, written in English (`Zielvariable` becomes "target variable").
- Add a rule to `openspec/config.yaml` that all artifacts and docs are written in English.
- PDFs and slides are committed to git (small files); the dataset is not.

## Capabilities

### New Capabilities
<!-- None: this is repository organization and documentation, not system behavior. -->

### Modified Capabilities
<!-- None. -->

This change sets `skip_specs: true` because it only introduces folder layout, documentation and tooling configuration; no spec-level behavior changes.

## Impact

- New directories: `notebooks/`, `data/`, `docs/` (and subfolders).
- Moved file: `LeadConversionRecord` becomes `docs/data/data-dictionary.md`.
- Modified files: `README.md`, `openspec/config.yaml`, new `.gitignore` for `data/`.
- No code, APIs or dependencies affected.
