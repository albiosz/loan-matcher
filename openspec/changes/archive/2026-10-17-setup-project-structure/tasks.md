# Tasks

## 1. Notebooks and data folders

- [x] 1.1 Create `notebooks/README.md` (purpose, numbering convention, run order) and `notebooks/examples/README.md` (read-only reference, copy before modifying); verify both files exist
- [x] 1.2 Create `data/README.md` explaining how to obtain the dataset and `data/.gitkeep`; verify `git status` lists them
- [x] 1.3 Add `.gitignore` that excludes `data/*` except `.gitkeep` and `README.md`, plus notebook checkpoints and Python caches; verify with `git check-ignore -v data/sample.csv` and that `data/README.md` is not ignored

## 2. Docs structure

- [x] 2.1 Create `docs/README.md` as a map of all docs subfolders; verify every link in it resolves to an existing path
- [x] 2.2 Create `docs/project/` with `brief.md`, `requirements.md`, `timeline.md` (short templates: goal, success metric, course requirements, milestones); verify files exist
- [x] 2.3 Create `docs/journal/README.md` (entry format `YYYY-MM-DD.md`: did / learned / next) and `docs/decisions/README.md` (format `NNN-title.md`: context / decision / consequence); verify files exist
- [x] 2.4 Create `docs/presentations/README.md` (one folder per milestone, source plus exported PDF) and `docs/references/` with `links.md` and `papers/README.md`, `course/README.md`; verify files exist

## 3. Data dictionary

- [x] 3.1 Create `docs/data/data-dictionary.md` from `LeadConversionRecord`, in English, as a table (field, type, meaning), marking `deducted_within_30_days` as the target variable; verify all 25 original fields are present
- [x] 3.2 Create `docs/data/eda-notes.md` stub and remove the root `LeadConversionRecord` file; verify the file is gone and no reference to it remains

## 4. Project-level docs and config

- [x] 4.1 Expand top-level `README.md` with a project summary (predicting loan lead conversion within 30 days), a folder map and the English-only rule; verify folder names in the README match the repo
- [x] 4.2 Add a `context` entry to `openspec/config.yaml` stating that all artifacts and docs are written in English; verify `openspec context --json` still succeeds

## 5. Integration check

- [x] 5.1 Compare the final tree against the layout in design.md and verify there are no missing or extra top-level folders

## 6. Agent entry point and spec alignment

- [x] 6.1 Trim `AGENTS.md` to a short pointer to `openspec/specs/` (structure and conventions) and `openspec/changes/` (work in flight); verify it contains no duplicated folder table and links to both paths
- [x] 6.2 Verify `CLAUDE.md` contains the `@AGENTS.md` import and `README.md` links to `AGENTS.md`
- [x] 6.3 Check the repository against each requirement in `specs/project-layout/spec.md` and verify `openspec validate setup-project-structure` passes
