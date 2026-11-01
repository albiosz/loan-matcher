# Notebooks

The notebooks are the main result of this project.

## Conventions

- Files are numbered in run/reading order: `01_eda.ipynb`, `02_baseline.ipynb`, ...
- Each notebook starts with a short goal and ends with a short conclusion.
- Course examples live in [`examples/`](examples/). Copy one to a numbered notebook before modifying it.
- Python packages used so far: `pandas`, `numpy`, `matplotlib`, `scipy`. There is no environment file yet; install them into `.venv` with `pip`.

## Stripping outputs (nbstripout)

Committed notebooks contain no cell outputs or execution counts. This keeps diffs readable and avoids merge conflicts. [`nbstripout`](https://github.com/kynan/nbstripout) does it as a git filter when you commit; your local files keep their outputs.

The filter is installed per clone (it lives in `.git/`, not in the repository), so set it up once after cloning:

```bash
source .venv/bin/activate
pip install nbstripout
nbstripout --install
```

### Verify it is active

```bash
git config --get filter.nbstripout.clean   # prints the nbstripout command
cat .git/info/attributes                   # contains: *.ipynb filter=nbstripout
git add notebooks/<name>.ipynb
git diff --cached notebooks/<name>.ipynb   # shows "outputs": [] and "execution_count": null
```

The filter command points at the Python interpreter that was active during `--install`. If you move or recreate `.venv`, run `nbstripout --install` again.

### Consequences

- Results are not stored in git, so notebooks must be reproducible by re-running them top to bottom. GitHub shows them without outputs. If a rendered result is needed, export it (HTML/PDF) to `docs/presentations/`.
- The filter applies to every `*.ipynb`, including those in [`examples/`](examples/); the examples' code and text are unchanged, only their outputs are dropped from commits.

## Index

| Notebook | Purpose |
|---|---|
| [`01_synthetic_data.ipynb`](01_synthetic_data.ipynb) | Simulates leads with the data dictionary's schema and writes `data/synthetic_leads.csv`; stand-in until real data is available |
| [`02_eda.ipynb`](02_eda.ipynb) | Explores a lead dataset (applicants, employment, income, loan, timing, conversion); set `COMPARE_PATH` to compare a second dataset, such as the real export, with verdicts per column |
