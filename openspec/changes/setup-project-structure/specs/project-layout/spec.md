# Spec Delta

## Purpose

Defines the repository layout and conventions of the loan-matcher project so that people and agents know where each kind of material belongs and keep following the same patterns.

## ADDED Requirements

### Requirement: Top-level layout
The repository SHALL contain the top-level folders `notebooks/`, `data/`, `docs/` and `openspec/`. Adding another top-level folder MUST be accompanied by an update to this spec and to `README.md`.

#### Scenario: New top-level folder proposed
- **WHEN** a change adds a top-level folder
- **THEN** the change updates this spec and `README.md` to include it

### Requirement: Numbered notebooks
Project notebooks SHALL live in `notebooks/` and be named `NN_name.ipynb`, numbered in run order, and each SHALL be listed in `notebooks/README.md`.

#### Scenario: New notebook added
- **WHEN** a notebook is added to `notebooks/`
- **THEN** its name starts with the next free two-digit number and it appears in the `notebooks/README.md` index

### Requirement: Course examples are read-only
Course examples SHALL be kept in `notebooks/examples/` and MUST NOT be modified in place; a copy is made to a numbered notebook before any changes.

#### Scenario: Modifying an example
- **WHEN** an example needs changes
- **THEN** it is copied to `notebooks/NN_name.ipynb` and the copy is edited

### Requirement: Data is never committed
Datasets MUST NOT be committed to git. `data/` SHALL be git-ignored except for `data/README.md` and `data/.gitkeep`, and `data/README.md` SHALL document how to obtain the data.

#### Scenario: Dataset file placed in data folder
- **WHEN** a dataset file is placed in `data/`
- **THEN** git ignores it

#### Scenario: Data README present
- **WHEN** the repository is cloned
- **THEN** `data/README.md` is present and explains how to obtain the dataset

### Requirement: Documentation layout
`docs/` SHALL contain `project/`, `data/`, `journal/`, `decisions/`, `presentations/` and `references/`. Journal entries SHALL be named `YYYY-MM-DD.md` and decision records `NNN-title.md`. Small PDFs and slides MAY be committed.

#### Scenario: Recording a work session
- **WHEN** a work session is documented
- **THEN** it is written to `docs/journal/YYYY-MM-DD.md` with Did, Learned and Next sections

#### Scenario: Recording a decision
- **WHEN** a model or approach is chosen
- **THEN** a record `docs/decisions/NNN-title.md` states context, decision and consequence

### Requirement: Data dictionary
`docs/data/data-dictionary.md` SHALL list every field of the dataset and identify the target variable.

#### Scenario: Looking up the target
- **WHEN** a reader opens the data dictionary
- **THEN** `deducted_within_30_days` is marked as the target variable

### Requirement: Folder index READMEs
Each project folder (`notebooks/`, `notebooks/examples/`, `data/`, `docs/` and its subfolders) SHALL contain a README or index file stating its purpose, and `docs/README.md` SHALL map all docs subfolders.

#### Scenario: Folder added to docs
- **WHEN** a docs subfolder is added
- **THEN** it has a README and is listed in `docs/README.md`

### Requirement: English language
All code, documents, notes and OpenSpec artifacts SHALL be written in English.

#### Scenario: Writing a new document
- **WHEN** a document or artifact is created
- **THEN** it is written in English

### Requirement: Agent entry point
`AGENTS.md` SHALL point agents to `openspec/specs/` as the source of truth for structure and conventions and to `openspec/changes/` for work in flight, and `CLAUDE.md` SHALL import `AGENTS.md`.

#### Scenario: Agent starts working in the repository
- **WHEN** an agent loads `CLAUDE.md` or `AGENTS.md`
- **THEN** it is directed to `openspec/specs/` and `openspec/changes/` for the layout and conventions
