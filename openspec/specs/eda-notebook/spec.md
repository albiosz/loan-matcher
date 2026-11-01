# eda-notebook Specification

## Purpose
Defines the exploratory data analysis notebook for loan leads: what it explores, how it can compare a second dataset (such as real company data) with the first, and the rules that keep it safe to run on personal data.

## Requirements

### Requirement: Parameterized datasets
The notebook SHALL be `notebooks/02_eda.ipynb` and expose in one parameter cell a required `DATA_PATH` (default `data/synthetic_leads.csv`) and an optional `COMPARE_PATH` (default none). Each dataset SHALL get a short label for plots and tables.

#### Scenario: Default run
- **WHEN** the notebook runs with its defaults
- **THEN** it explores `data/synthetic_leads.csv` only and performs no comparison

#### Scenario: Comparison run
- **WHEN** `COMPARE_PATH` points to a second dataset
- **THEN** the notebook explores both datasets and adds the comparison section

#### Scenario: Only paths change
- **WHEN** a real export in the dictionary's schema is given as `DATA_PATH` or `COMPARE_PATH`
- **THEN** the notebook runs without changes to its logic

### Requirement: Schema validation on load
The notebook SHALL check each loaded dataset against the field list in `docs/data/data-dictionary.md` and MUST stop with a message naming the missing or extra columns before any analysis if they differ.

#### Scenario: Matching columns
- **WHEN** a dataset has exactly the dictionary's columns
- **THEN** analysis proceeds

#### Scenario: Column missing in the comparison dataset
- **WHEN** the dataset in `COMPARE_PATH` lacks a dictionary column
- **THEN** the notebook stops and names the missing column, and no plots or verdicts are produced

### Requirement: Single-dataset exploration topics
The notebook SHALL report an overview (rows, columns, missing values per column), the applicant profile (age, applicants, children, living arrangements, occupation, education), employment and residence (employment duration in years, probation, mini job, temporary job, residence duration), income and existing credit, the requested loan (amount, duration, purpose), and the conversion rate overall and by segment.

#### Scenario: Age and tenure shown in years
- **WHEN** the reader looks at employment duration and residence duration
- **THEN** they are shown in years, with the conversion from the months in the data stated

#### Scenario: Loan distribution
- **WHEN** the reader looks at the loan section
- **THEN** it shows the distribution of loan amount and duration and the share of leads per loan purpose

#### Scenario: Conversion by segment
- **WHEN** the reader looks at the target section
- **THEN** it shows the overall conversion rate and the conversion rate by loan purpose, age group, disposable income group and employment flags, and says that the target is rare

### Requirement: Affordability view
The notebook SHALL show the approximate monthly payment of the requested loan (amount divided by duration, ignoring interest) and its ratio to disposable income against conversion, for exploration only, without saving derived columns.

#### Scenario: Payment ratio and conversion
- **WHEN** the reader looks at the affordability view
- **THEN** conversion rate is shown across payment-to-disposable-income groups

#### Scenario: Nothing is saved
- **WHEN** the notebook has run
- **THEN** no file in `data/` has been created or changed

### Requirement: Lead timing
The notebook SHALL show lead volume per hour of day, weekday and month, and for each dataset separately. It MUST NOT overlay `prediction_at` between datasets.

#### Scenario: Timing in comparison mode
- **WHEN** both datasets are given
- **THEN** timing is shown once per dataset, not as an overlay

### Requirement: Comparison plots
When `COMPARE_PATH` is set, the notebook SHALL show every distribution with both datasets in one plot, using shares or densities rather than counts. Numeric columns SHALL be plotted as overlaid distributions and categorical columns as side-by-side shares.

#### Scenario: Different dataset sizes
- **WHEN** one dataset has far fewer rows than the other
- **THEN** the plots remain comparable because they show shares or densities

### Requirement: Column distance scores
When `COMPARE_PATH` is set, the notebook SHALL score each field except `application_id` and `prediction_at` by distance between the datasets: the Kolmogorov–Smirnov statistic for numeric fields and the total variation distance for categorical fields, each between 0 and 1, computed on non-missing values.

#### Scenario: Identical datasets
- **WHEN** the same file is given as `DATA_PATH` and `COMPARE_PATH`
- **THEN** every score is 0

#### Scenario: Unit mismatch
- **WHEN** one dataset's income is 12 times the other's
- **THEN** the income score is close to 1

#### Scenario: Excluded fields
- **WHEN** scores are produced
- **THEN** `application_id` and `prediction_at` are not scored

#### Scenario: Field empty in one dataset
- **WHEN** a field has no non-missing values in one of the datasets
- **THEN** it gets no score and the verdict "not comparable"

### Requirement: Verdicts and summary
When `COMPARE_PATH` is set, the notebook SHALL give each scored field a verdict from configurable thresholds, defaulting to "similar" below 0.10, "moderate shift" from 0.10 below 0.20 and "shifted" from 0.20, and SHALL show a table sorted by score and a one-line summary naming the shifted fields.

#### Scenario: Same-generator check
- **WHEN** the synthetic dataset is compared with one regenerated with a different seed
- **THEN** every field is "similar"

#### Scenario: Thresholds changed
- **WHEN** the threshold parameters are changed
- **THEN** the verdicts follow the new thresholds without other code changes

#### Scenario: Summary line
- **WHEN** some fields are "shifted"
- **THEN** the summary names them, and when none are it says so

#### Scenario: Verdict is not a judgment of quality
- **WHEN** a reader reads the comparison section
- **THEN** the notebook states that a verdict flags a difference to inspect, not an error in either dataset

### Requirement: Conversion rate and missing values compared
When `COMPARE_PATH` is set, the notebook SHALL show for each dataset its conversion rate, conversion by loan purpose and missing-value share per column, next to each other.

#### Scenario: Conversion rates differ
- **WHEN** the datasets' conversion rates differ
- **THEN** both rates are shown with their absolute and relative difference

### Requirement: Aggregate-only output
The notebook MUST NOT display individual rows or identifiers of the datasets it reads. Outputs SHALL be aggregates (counts, shares, summary statistics, plots).

#### Scenario: Previewing data
- **WHEN** the notebook shows what a dataset looks like
- **THEN** it shows column types and summary statistics, not raw rows

### Requirement: Findings and limitations are stated
The notebook SHALL start with a short goal and end with a short conclusion. Both SHALL state that, for synthetic data, the relationships shown are assumptions of the generator and say nothing about real leads.

#### Scenario: Reader looks for limitations
- **WHEN** a reader opens the notebook
- **THEN** its introduction and conclusion state that synthetic results do not describe real-data behavior

### Requirement: Notebook is indexed and reproducible
The notebook SHALL be listed in `notebooks/README.md` and SHALL run top to bottom from a clean kernel without manual steps, reading data only from CSV files.

#### Scenario: Index entry
- **WHEN** `notebooks/README.md` is read
- **THEN** it lists `02_eda.ipynb` with its purpose

#### Scenario: Clean run
- **WHEN** the notebook is run top to bottom from a clean kernel with the synthetic file present
- **THEN** it completes without errors
