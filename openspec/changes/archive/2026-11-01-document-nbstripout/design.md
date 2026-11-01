# Design

## Context

Observed in the current clone: `nbstripout --install` wrote the `filter "nbstripout"` and `diff "ipynb"` sections to `.git/config` (clean filter, `smudge = cat`, `required = true`, and a `textconv` for diffs), and the attribute lines (`*.ipynb filter=nbstripout`, `*.ipynb diff=ipynb`) to `.git/info/attributes`. Both files are local to the clone and never pushed. The filter command uses an absolute path to this clone's `.venv` interpreter. There is no `.gitattributes` and no requirements file. See proposal.md for motivation.

## Goals / Non-Goals

**Goals:**
- A fresh clone can reproduce the current setup from the docs alone, in two commands.
- The trade-offs of stripped outputs are written down where notebook authors read them.

**Non-Goals:**
- Enforcing the filter (hooks, CI). With a solo project, documentation is enough for now.
- Changing the existing local install.

## Decisions

**Document the local install, do not version it.** The setup stays `nbstripout --install` (writes to `.git/config` and `.git/info/attributes`). Alternative: commit a `.gitattributes` with `*.ipynb filter=nbstripout`. Rejected for now: the filter definition still has to be installed per clone, and if it is missing, git ignores the attribute silently, so it adds little. It also adds a tracked file that the specs would need to cover. It can be proposed later together with `required = true` enforcement.

**Put the instructions in `notebooks/README.md`, with a pointer from `README.md`.** That is where the notebook conventions already live. The root README gets a short "Setup" line only, to avoid duplicating the steps.

**Include a verification step.** The filter fails silently when missing (for example after recreating `.venv`, because the filter points at that interpreter). The docs give a check with `git config --get filter.nbstripout.clean` and a test of staged content with `git diff --cached`, plus a note to re-run `nbstripout --install` if the venv moves.

**Install into the project virtualenv.** The docs use `pip install nbstripout` inside the activated `.venv`, matching the current setup. Alternative: global install with `pipx`. Rejected: the filter would depend on a tool outside the project environment.

## Risks / Trade-offs

- [Outputs are not in git, so GitHub shows notebooks without results] → Notebooks must be reproducible by re-running; the docs say so. If a rendered deliverable is needed, export it (HTML/PDF) into `docs/presentations/`.
- [Filter also strips `notebooks/examples/`, so committed course examples lose their outputs] → Accepted; the examples' content is unchanged, and the docs mention it.
- [Moving or recreating `.venv` breaks the filter path] → The docs include the verification step and the re-install command.
- [Filter missing in a new clone silently commits outputs] → Documented in the root README; hooks/CI left for a later change.
