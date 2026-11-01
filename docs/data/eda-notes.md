# EDA notes

Findings from exploratory data analysis. Link to the notebook that produced each finding.

## Synthetic leads (`data/synthetic_leads.csv`)

Source: [`notebooks/02_eda.ipynb`](../../notebooks/02_eda.ipynb). All of this describes the generator's assumptions, not real leads.

- 50,000 leads over 2025; 3.0% convert, so accuracy is not a useful metric (always predicting 0 is right 97% of the time).
- Applicants are 18 to 75 years old (mean 41); 14.5% are not currently employed, 3.9% in probation, 5.5% have a mini job. 11% have no disposable income and 55% have open credit.
- Median loan: 7,300 EUR over 36 months. New car (median 14,800 EUR) and debt conversion (10,600 EUR) are the largest, electronics (2,700 EUR) the smallest.
- Conversion by purpose: highest `dispo` (4.2%) and `debt_conversion` (3.8%), lowest `journey` (1.9%) and `other` (2.0%).
- Conversion is near zero without disposable income (0.3%) and when the monthly payment exceeds the disposable income (0.5%); it is about 1.1% for probation and mini jobs, and lower for ages 18-24 (1.5%) and 65+ (2.0%) than for ages 25-64 (2.9 to 3.3%).
- Leads arrive mostly 08:00-20:00 (81%), evenly across weekdays and months.
