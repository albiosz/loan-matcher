# synthetic-data-generator Specification

## Purpose

Defines the synthetic lead data generator that stands in for the company's real data until access is granted: which data it produces, how conversion is modeled, which assumptions are intentionally unvalidated, and how real data replaces it.

## Requirements

### Requirement: Schema follows the data dictionary
The generator SHALL produce exactly the fields listed in `docs/data/data-dictionary.md`, with the same names and in the same order, and MUST fail instead of writing output when a field is missing or extra.

#### Scenario: Dictionary and generator agree
- **WHEN** the generator runs and all dictionary fields are produced
- **THEN** the output file has those fields, in dictionary order, and no others

#### Scenario: Field added to the dictionary only
- **WHEN** a field is added to the data dictionary but the generator does not produce it
- **THEN** the run fails with a schema mismatch and writes no output

### Requirement: Output location and privacy
The generator SHALL be a numbered notebook in `notebooks/` that writes one CSV file to the git-ignored `data/` folder, with one row per application, and MUST NOT use or contain real customer data.

#### Scenario: Output is not committed
- **WHEN** the generator writes its CSV to `data/`
- **THEN** git ignores the file

#### Scenario: No real data involved
- **WHEN** the generator runs
- **THEN** every value is drawn from the simulation and none is read from a company export

### Requirement: Reproducible with documented settings
The generator SHALL expose the random seed, number of rows, target conversion rate and outcome noise level as settings, and the same settings SHALL produce identical output.

#### Scenario: Same seed twice
- **WHEN** the generator runs twice with the same settings
- **THEN** both outputs are identical

#### Scenario: Different seed
- **WHEN** the seed changes and other settings stay the same
- **THEN** the data differs but keeps the same schema and approximately the same conversion rate

### Requirement: Units and field semantics are stated
The generator documentation SHALL state its assumed units and field semantics: money in EUR per month, durations in months, age in years, flags as 0/1, `main_income` as the first applicant's net income, `other_income` as all remaining income, `disposable_income` as incomes minus total expenses minus open credit installments, and `employment_duration` of 0 as not currently employed.

#### Scenario: Disposable income identity
- **WHEN** any row is checked
- **THEN** `disposable_income` equals `main_income + other_income - total_expenses - open_credit_monthly_installments`

#### Scenario: Reader checks the units
- **WHEN** a reader opens the generator notebook
- **THEN** the assumed units are stated in its introduction

### Requirement: Loan purposes follow the data dictionary
`loan_purpose` SHALL only take values from the "Loan purposes" list in the data dictionary, and the generator MUST fail if its own list of purposes differs from that list.

#### Scenario: Valid purposes only
- **WHEN** the generator writes its output
- **THEN** every `loan_purpose` value is one of the documented purposes

#### Scenario: Purpose list changed in the dictionary
- **WHEN** a purpose is added to or removed from the dictionary but not from the generator
- **THEN** the run fails with a purpose mismatch

### Requirement: Applicant and household plausibility
Generated records SHALL be internally plausible: applicants are at least 18 years old, `relationship_between_applicants` is present only when `two_applicants` is 1, and employment and residence durations do not exceed what the applicant's age allows.

#### Scenario: Single applicant
- **WHEN** `two_applicants` is 0
- **THEN** `relationship_between_applicants` is empty

#### Scenario: Tenure bounded by age
- **WHEN** a row has a non-missing `employment_duration`
- **THEN** it does not exceed the months since the applicant turned 18, except that an employed applicant always has at least 1 month

#### Scenario: Retirement follows age
- **WHEN** a row has occupation group `retired`
- **THEN** the applicant is 63 or older

### Requirement: Mini jobs and regular employment
`mini_job` SHALL be 1 only for applicants whose occupation group has no regular job (retired, unemployed or student, homemaker), and `in_probation` SHALL be 1 only for employed applicants.

#### Scenario: Mini job with regular occupation
- **WHEN** the occupation group is blue collar, white collar, civil servant or self-employed
- **THEN** `mini_job` is 0

#### Scenario: Probation without employment
- **WHEN** `employment_duration` is 0
- **THEN** `in_probation` is 0

#### Scenario: Mini job counts as employment
- **WHEN** `mini_job` is 1
- **THEN** `employment_duration` is at least 1

### Requirement: Time of leads
`prediction_at` SHALL cover one year, with lead volume concentrated in daytime hours and spread evenly over days of the week, and `application_id` SHALL increase with `prediction_at`.

#### Scenario: Ids follow time
- **WHEN** rows are sorted by `application_id`
- **THEN** `prediction_at` is non-decreasing

#### Scenario: Daytime concentration
- **WHEN** leads are counted by hour of day
- **THEN** most leads fall between 08:00 and 20:00 and few fall between 22:00 and 06:00

#### Scenario: Time-based split possible
- **WHEN** the data is split at a point in time
- **THEN** both parts contain leads and every lead in the later part is dated after every lead in the earlier part

### Requirement: Conversion model
`deducted_within_30_days` SHALL be generated from observed fields plus unobserved noise: a lead converts more often when it can afford and qualify for the loan (eligibility) and when it wants the loan (intent). Need, such as a tight budget or existing credit, SHALL raise intent while lowering eligibility.

#### Scenario: Unaffordable loan
- **WHEN** the new loan's monthly payment clearly exceeds the lead's disposable income
- **THEN** the lead converts far less often than the average lead

#### Scenario: Need versus eligibility
- **WHEN** conversion rate is compared across disposable income groups
- **THEN** low-to-middle income groups convert at least as often as high-income groups, and high-income groups still convert

#### Scenario: Purpose effect
- **WHEN** conversion rate is compared across loan purposes
- **THEN** acute-need purposes (`dispo`, `debt_conversion`) convert more often than `journey` and `other`

#### Scenario: Employment instability
- **WHEN** a lead is in probation or has a mini job
- **THEN** its conversion rate is lower than that of other leads

### Requirement: Calibrated conversion rate
The generator SHALL calibrate the outcome so the overall conversion rate matches the target rate setting, which SHALL default to 3% (business estimate), while keeping the outcome non-deterministic.

#### Scenario: Default rate
- **WHEN** the generator runs with default settings
- **THEN** the share of rows with `deducted_within_30_days` equal to 1 is within 0.3 percentage points of 3%

#### Scenario: Outcome stays uncertain
- **WHEN** a model is trained on the generated data
- **THEN** its ROC AUC stays clearly below 1, because leads with similar features have different outcomes

### Requirement: Missing values
The generator SHALL introduce missing values at fixed per-column rates in education, residence duration, employment duration, number of children and occupation group, and MUST NOT leave identifiers, `prediction_at` or the target missing.

#### Scenario: Configured columns have gaps
- **WHEN** the output is inspected
- **THEN** each configured column has missing values near its configured rate

#### Scenario: Protected columns complete
- **WHEN** the output is inspected
- **THEN** `application_id`, `prediction_at` and `deducted_within_30_days` have no missing values

### Requirement: Built-in sanity checks
The generator notebook SHALL report the conversion rate, conversion by loan purpose, by disposable income group and by employment flags, and the score of a baseline model on a time-based split next to the score of the noise-free signal used by the simulator.

#### Scenario: Reader assesses the data
- **WHEN** the notebook has run
- **THEN** it shows the overall conversion rate, the grouped conversion rates and both scores

### Requirement: Synthetic results are labeled as a pipeline check
The generator notebook SHALL state that all relationships are assumptions and that model scores on synthetic data confirm the pipeline works but MUST NOT be presented as expected performance on real leads.

#### Scenario: Reader looks for limitations
- **WHEN** a reader opens the notebook
- **THEN** its introduction and conclusion state that synthetic scores say nothing about real-data performance

### Requirement: Real data is a drop-in replacement
The generator SHALL be the only place that creates synthetic data, and analysis and modeling notebooks SHALL read only a CSV file from `data/` with the data dictionary's schema, so a real export with the same columns replaces the synthetic file without code changes beyond the file path.

#### Scenario: Real export arrives
- **WHEN** a real export with the dictionary's column names is placed in `data/` and the loading path is updated
- **THEN** the analysis and modeling notebooks run without changes to their logic

#### Scenario: Assumptions reviewed
- **WHEN** real data becomes available
- **THEN** the unit assumptions, conversion rate and purpose distribution are compared with it and the specification is updated where they differ
