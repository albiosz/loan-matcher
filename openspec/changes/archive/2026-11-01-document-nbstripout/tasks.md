# Tasks

## 1. Notebook setup documentation

- [x] 1.1 Add a "Stripping outputs (nbstripout)" section to `notebooks/README.md` covering why outputs are stripped, the one-time setup (`pip install nbstripout` in the project `.venv`, then `nbstripout --install`), and the examples/reproducibility trade-off; verify by following the steps in a throwaway clone and checking that the filter is registered
- [x] 1.2 Add the verification step to that section (`git config --get filter.nbstripout.clean`, and checking staged content with `git diff --cached`) plus the note to re-run `nbstripout --install` after recreating `.venv`; verify by running the documented commands in this clone and confirming the output matches the docs

## 2. Root README pointer

- [x] 2.1 Add a short "Setup" section to `README.md` linking to the nbstripout section in `notebooks/README.md`; verify the link resolves and the README structure table is unchanged

## 3. Integration check

- [x] 3.1 Stage a notebook that has outputs in the throwaway clone and verify `git diff --cached` shows no `outputs` or `execution_count` values
- [x] 3.2 Run `openspec validate document-nbstripout` and verify it passes

## Workflow follow-up

- Archive the change with `/opsx:archive` so the new requirement is synced into `openspec/specs/project-layout/spec.md`.
