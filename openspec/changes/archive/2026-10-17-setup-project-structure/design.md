# Design

## Context

The repository currently holds only `README.md`, a `LeadConversionRecord` file (a plain-text list of feature names with the target `deducted_within_30_days`) and OpenSpec/IDE config. It is a solo project; the notebooks are the graded result. See proposal.md for motivation.

## Goals / Non-Goals

**Goals:**
- One obvious place for each kind of material: own work, collected references, data.
- Make the notebooks the visible centre of the repo.
- Keep a lightweight trail of what was done and why (journal, decisions).

**Non-Goals:**
- No Python package layout (`src/`), environment files or CI; added only when needed.
- No task tracker or meeting notes (solo work; OpenSpec covers planned work).
- No data pipeline or notebook content in this change.

## Decisions

**Layout** (top level: `notebooks/`, `data/`, `docs/`, `openspec/`):

```
notebooks/   README.md, NN_name.ipynb, examples/
data/        .gitkeep, README note on how to obtain data
docs/        README.md
  project/        brief.md, requirements.md, timeline.md
  data/           data-dictionary.md, eda-notes.md
  journal/        YYYY-MM-DD.md
  decisions/      NNN-title.md
  presentations/  one folder per milestone
  references/     links.md, papers/, course/
```

- **Notebooks at top level, not under `docs/`.** They are the product, not documentation. Alternative (`docs/notebooks/`) hides the main deliverable.
- **Numbered notebooks (`01_`, `02_`).** Gives a readable top-to-bottom story. Alternative: free names, which lose run order.
- **Course examples in `notebooks/examples/`**, copied to a numbered notebook before modification. Keeps the reference intact and next to the work.
- **Collected vs own work separated.** `docs/references/` is input from others; `journal/`, `decisions/`, `presentations/` are own output. Avoids a single mixed dump.
- **`data/` ignored by git by default.** Loan applications are personal data and may be large. A `.gitignore` excludes `data/*` except `.gitkeep` and a README documenting how to get the data. Alternative: commit the dataset; deferred until its size and sensitivity are known (the user has not confirmed).
- **PDFs and slides committed.** Small files; keeps everything in one place and works offline.
- **`LeadConversionRecord` becomes `docs/data/data-dictionary.md`**, translated to English, with columns for type and meaning left to fill in. The original root file is removed so there is one source of truth.
- **English everywhere**, enforced for OpenSpec artifacts via a `context` entry in `openspec/config.yaml`.
- **`AGENTS.md` is a thin pointer, `CLAUDE.md` imports it.** The layout and conventions live in the `project-layout` spec (`openspec/specs/`), so agent files only say where to look (`openspec/specs/` for structure, `openspec/changes/` for work in flight). Alternative: a full folder map in `AGENTS.md`, which duplicates the spec and drifts. `CLAUDE.md` is a one-line `@AGENTS.md` import so there is one agent file to maintain.
- **Each folder gets a short README** stating its purpose, so the structure explains itself.

## Risks / Trade-offs

- [Structure outgrows the project] → It is intentionally small; unused folders hold only a README and can be deleted.
- [Dataset may later need to be committed] → Remove the `data/` ignore rule; nothing else depends on it.
- [Empty folders clutter the repo] → Only folders with a README or `.gitkeep` are created.
