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
- Add `AGENTS.md` (short pointer to `openspec/specs/` and `openspec/changes/`) and `CLAUDE.md` (imports `AGENTS.md`) so agents follow the structure.
- Capture the layout and conventions as the `project-layout` spec, so it stays the durable source of truth after archive.
- PDFs and slides are committed to git (small files); the dataset is not.

## Capabilities

### New Capabilities
- `project-layout`: repository folder structure and conventions (notebooks, data, docs, language, index READMEs, agent entry point)

### Modified Capabilities
<!-- None. -->

## Impact

- New directories: `notebooks/`, `data/`, `docs/` (and subfolders).
- Moved file: `LeadConversionRecord` becomes `docs/data/data-dictionary.md`.
- New files: `AGENTS.md`, `CLAUDE.md`, `.gitignore`, new spec `openspec/specs/project-layout/` (after archive).
- Modified files: `README.md`, `openspec/config.yaml`.
- No code, APIs or dependencies affected.
