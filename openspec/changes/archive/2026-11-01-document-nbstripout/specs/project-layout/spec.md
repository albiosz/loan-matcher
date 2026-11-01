# Spec Delta

## ADDED Requirements

### Requirement: Notebook outputs are not committed
Committed notebooks SHALL NOT contain cell outputs or execution counts. Output stripping SHALL be done with `nbstripout` installed as a git filter in each clone, and `notebooks/README.md` SHALL document the one-time setup and how to verify it.

#### Scenario: Committing a notebook with outputs
- **WHEN** a notebook with cell outputs is committed in a clone where the filter is installed
- **THEN** the committed version contains no outputs or execution counts

#### Scenario: Setting up a fresh clone
- **WHEN** someone clones the repository
- **THEN** `README.md` points to the `notebooks/README.md` section that explains how to install the filter

#### Scenario: Verifying the filter
- **WHEN** a contributor follows the verification step in `notebooks/README.md`
- **THEN** they can tell whether the filter is active in their clone
