# Proposal

## Why

`nbstripout` is already installed in the author's local clone: a git clean filter strips notebook outputs and execution counts on commit, and a diff textconv keeps `git diff` readable. That setup lives only in `.git/config` and `.git/info/attributes`, so it is not versioned. A fresh clone, a new machine or an agent has no filter, and notebooks with outputs can silently end up in commits. The convention is undocumented and not in the specs.

## What Changes

- Add a requirement to the `project-layout` spec: committed notebooks have outputs stripped via `nbstripout`, and the setup steps are documented.
- Document the one-time setup (`pip install nbstripout`, `nbstripout --install`) and how to verify it in `notebooks/README.md`.
- Add a short "Setup" pointer in the root `README.md` so a fresh clone finds the step.
- Document the trade-off: outputs are not in git, so results must be reproducible by re-running the notebooks, and the filter also affects `notebooks/examples/`.

Out of scope: tracking the setup through a committed `.gitattributes`, pre-commit hooks, CI checks, and dependency management (the repo has no requirements file yet).

## Capabilities

### New Capabilities

None.

### Modified Capabilities

- `project-layout`: adds a requirement that committed notebooks are stripped of outputs with `nbstripout` and that the setup is documented in `notebooks/README.md`.

## Impact

- `openspec/specs/project-layout/spec.md` (via delta, synced on archive)
- `notebooks/README.md`, `README.md`
- No code, notebooks or git configuration change; the local `nbstripout` install stays as is.
