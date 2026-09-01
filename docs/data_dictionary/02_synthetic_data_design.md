# Synthetic Data Design

## 1. Purpose

This document defines the logic, scale, distributions, dependencies, and controlled anomalies that will be used to generate the synthetic datasets for the Project Control Analytics Platform.

The objective is to create realistic project-control data that supports data engineering, analytics, data-quality validation, and machine-learning use cases.

## 2. Simulation Scope

The first synthetic dataset will include:

- 25 projects.
- 4 to 12 work packages per project.
- 3 years of project history.
- 150 to 250 resources.
- Monthly budgets and forecasts.
- Daily or transaction-level actual costs.
- Commitments and supplier invoices.
- Timesheet records.
- Multiple forecast versions.
- Controlled data-quality issues.
- A subset of projects and work packages with realistic cost-overrun behavior.

## 3. Project Profiles

Projects will vary by:

- Total planned duration.
- Number of work packages.
- Number of assigned resources.
- Cost structure.
- Complexity.
- Client.
- Country.
- Discipline mix.
- Probability of schedule delay.
- Probability of cost overrun.

### Initial project complexity categories

- LOW
- MEDIUM
- HIGH

Project complexity will influence:

- Number of work packages.
- Number of resources.
- Budget size.
- Forecast volatility.
- Probability of cost overrun.
- Probability of schedule delay.

## 4. Project Duration

Suggested duration ranges:

| Complexity | Duration |
|---|---|
| LOW | 6-12 months |
| MEDIUM | 12-24 months |
| HIGH | 18-36 months |

## 5. Work Package Distribution

Each project will contain between 4 and 12 work packages.

Possible disciplines:

- Civil
- Structural
- Mechanical
- Electrical
- Process
- Project Controls
- Procurement
- Construction Support
- Environmental
- Other

Higher-complexity projects will generally contain more work packages and a wider discipline mix.

## 6. Budget Generation

Project budgets will be generated based on:

- Project complexity.
- Project duration.
- Number of work packages.
- Discipline mix.

Budget will be allocated across:

- Work packages.
- Financial periods.
- Cost categories.

### Initial cost-category distribution

Typical project cost structure:

- LABOR: 35%-60%
- SUBCONSULTANT: 10%-30%
- TRAVEL: 1%-5%
- MATERIALS: 5%-20%
- EQUIPMENT: 2%-15%
- OTHER: 1%-5%

These ranges will vary by project type and complexity.

## 7. Actual Cost Behavior

Actual costs will follow the budget profile but will include controlled variation.

Possible behaviors:

### Normal project

Actual costs remain relatively close to the planned budget curve.

### Cost-overrun project

The project begins relatively close to budget but develops cost pressure over time.

Possible drivers:

- Increased labor hours.
- Higher-than-planned hourly rates.
- Additional subconsultant costs.
- Material cost increases.
- Schedule extensions.
- Late scope changes.
- Increased commitments.
- Forecast revisions.

### Underrun project

Actual costs remain below the original budget because of:

- Lower resource utilization.
- Reduced scope.
- Lower supplier costs.
- Early completion.

## 8. Cost Overrun Logic

Projects will not be labeled as overrun randomly.

Cost-overrun risk will be driven by variables such as:

- Budget consumption versus elapsed project time.
- Actual hours versus planned hours.
- Forecast growth across reporting periods.
- Commitment growth.
- High monthly burn rate.
- Schedule delay.
- Repeated forecast revisions.
- High subconsultant exposure.

This creates learnable patterns for future machine-learning models.

## 9. Forecast Generation

Forecasts will be generated monthly.

Each reporting period will create a new forecast version.

Example:

- FCST_2026_01
- FCST_2026_02
- FCST_2026_03

Previous forecast versions will remain unchanged.

Forecasts for high-risk projects will generally increase over time as cost pressure becomes visible.

Stable projects will show relatively small forecast changes.

## 10. Resource Generation

The dataset will initially contain approximately 150-250 resources.

Resources will vary by:

- Discipline.
- Role.
- Employment type.
- Hourly rate.
- Active dates.

Hourly rates will depend on:

- Role seniority.
- Discipline.
- Employment type.

Example role levels:

- Junior
- Intermediate
- Senior
- Lead
- Manager

## 11. Timesheet Generation

Timesheets will be generated based on:

- Resource assignments.
- Active project periods.
- Work-package schedules.
- Resource availability.
- Project workload.

Most resources will record between 6 and 9 hours on active working days.

Overrun projects may show:

- Higher overtime.
- Higher hours than originally forecast.
- Extended resource activity near project completion.

## 12. Commitments and Invoices

Commitments will primarily represent:

- Subconsultant contracts.
- Purchase orders.
- Service agreements.

Invoices will be generated against a subset of commitments.

Commitments may experience:

- Contract increases.
- Partial actualization.
- Full actualization.
- Cancellation.

High-risk projects may experience a greater frequency of commitment increases.

## 13. Data Quality Issues

A controlled percentage of raw records will contain intentional quality problems.

Potential issues include:

- Missing project IDs.
- Invalid work-package IDs.
- Duplicate invoice lines.
- Duplicate source references.
- Incorrect labor-cost calculations.
- Incorrect remaining-commitment calculations.
- Invalid financial periods.
- Inconsistent currency codes.
- Invalid date sequences.
- Negative values without a valid reversal transaction.
- Forecast duplicates.
- Timesheets outside resource active dates.

These errors will be introduced only in the raw layer.

The validation pipeline will be expected to detect and isolate them.

## 14. Initial Data Quality Rates

Suggested starting proportions:

- 96%-98% valid records.
- 2%-4% controlled anomalies.

The objective is not to make the dataset unrealistic, but to include enough issues to demonstrate data-quality controls.

## 15. Reproducibility

Synthetic-data generation must be reproducible.

The generation script will use a fixed random seed.

Example:

random_seed = 42

This ensures that the same dataset can be regenerated during testing and development.

## 16. Expected Scale

Approximate first-version dataset size:

| Dataset | Approximate Rows |
|---|---:|
| projects | 25 |
| work_packages | 150-220 |
| resources | 150-250 |
| budget | 5,000-10,000 |
| forecasts | 20,000-40,000 |
| timesheets | 50,000-100,000 |
| actual_costs | 15,000-30,000 |
| commitments | 500-1,500 |
| invoices | 2,000-5,000 |
| calendar | ~1,100 |

The scale may be adjusted after performance testing.
