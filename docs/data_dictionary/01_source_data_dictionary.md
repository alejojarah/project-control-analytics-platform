# Source Data Dictionary

This document defines the structure of the synthetic source datasets used by the Project Control Analytics Platform.

## 1. projects.csv

**Granularity:** One row per project.

| Column | Description | Type | Required | Example | Key | Validation |
|---|---|---|---|---|---|---|
| project_id | Unique project identifier | String | Yes | PRJ-001 | PK | Unique and not null |
| project_name | Project name | String | Yes | Metro Extension Program | | Not empty |
| client_name | Client or organization | String | Yes | Metro Authority | | Not empty |
| start_date | Planned project start date | Date | Yes | 2026-01-15 | | Must be before planned_end_date |
| planned_end_date | Planned completion date | Date | Yes | 2028-12-31 | | Must be after start_date |
| project_status | Current project status | String | Yes | ACTIVE | | Allowed values only |
| currency_code | Project currency | String | Yes | CAD | | Three-letter currency code |
| project_manager | Responsible project manager | String | Yes | Laura Smith | | Not empty |
| country_code | Country where the project is executed | String | Yes | CA | | Valid country code |
| created_at | Record creation timestamp | Datetime | Yes | 2026-01-01 09:30:00 | | Valid timestamp |

### Allowed project statuses

- PLANNED
- ACTIVE
- ON_HOLD
- CLOSED
- CANCELLED

## 2. work_packages.csv

**Granularity:** One row per work package.

| Column | Description | Type | Required | Example | Key | Validation |
|---|---|---|---|---|---|---|
| work_package_id | Unique work package identifier | String | Yes | WP-001-01 | PK | Unique and not null |
| project_id | Parent project identifier | String | Yes | PRJ-001 | FK | Must exist in projects.csv |
| work_package_name | Work package name | String | Yes | Civil Design | | Not empty |
| discipline | Main discipline | String | Yes | Civil | | Allowed category |
| planned_start_date | Planned start date | Date | Yes | 2026-02-01 | | Must be within project dates |
| planned_end_date | Planned finish date | Date | Yes | 2026-11-30 | | Must be after planned_start_date |
| work_package_status | Current work package status | String | Yes | ACTIVE | | Allowed values only |
| manager_name | Work package manager | String | Yes | Daniel Ruiz | | Not empty |
| cost_center | Internal cost-center code | String | No | CC-4102 | | Standard format |
| created_at | Record creation timestamp | Datetime | Yes | 2026-01-10 10:15:00 | | Valid timestamp |

### Allowed work package statuses

- PLANNED
- ACTIVE
- ON_HOLD
- COMPLETED
- CANCELLED

## 3. budget.csv

**Granularity:** One row per project, work package, financial period, cost category, and budget version.

| Column | Description | Type | Required | Example | Key | Validation |
|---|---|---|---|---|---|---|
| budget_id | Unique budget record identifier | String | Yes | BUD-000001 | PK | Unique and not null |
| project_id | Project identifier | String | Yes | PRJ-001 | FK | Must exist in projects.csv |
| work_package_id | Work package identifier | String | Yes | WP-001-01 | FK | Must exist in work_packages.csv and belong to the same project |
| financial_period | Period to which the budget applies | String | Yes | 2026-01 | | Must follow YYYY-MM format |
| cost_category | Type of cost budgeted | String | Yes | LABOR | | Allowed values only |
| budget_amount | Budgeted amount for the period and category | Decimal | Yes | 250000.00 | | Must be greater than or equal to zero |
| currency_code | Currency of the budget amount | String | Yes | CAD | | Must match project currency in the first version |
| budget_version | Budget baseline or revision identifier | String | Yes | BASELINE_V1 | | Must not be empty |
| created_at | Record creation timestamp | Datetime | Yes | 2026-01-05 08:00:00 | | Valid timestamp |

### Allowed cost categories

- LABOR
- SUBCONSULTANT
- TRAVEL
- MATERIALS
- EQUIPMENT
- OTHER

### Initial business rules for budget data

- Every budget record must belong to an existing project and work package.
- The work package must belong to the project referenced by the record.
- Budget amounts cannot be negative.
- Historical budget versions must not be overwritten.
- The same combination of project, work package, financial period, cost category, and budget version must not be duplicated.
- In the first version of the project, budget currency must match the project currency.

## 4. actual_costs.csv

**Granularity:** One row per posted financial transaction.

| Column | Description | Type | Required | Example | Key | Validation |
|---|---|---|---|---|---|---|
| actual_cost_id | Unique actual cost record identifier | String | Yes | ACT-000001 | PK | Unique and not null |
| project_id | Project identifier | String | Yes | PRJ-001 | FK | Must exist in projects.csv |
| work_package_id | Work package identifier | String | Yes | WP-001-01 | FK | Must exist in work_packages.csv and belong to the same project |
| transaction_date | Date when the cost transaction occurred | Date | Yes | 2026-02-15 | | Must be a valid date |
| posting_date | Date when the transaction was posted | Date | Yes | 2026-02-18 | | Must be equal to or after transaction_date |
| financial_period | Financial period assigned to the transaction | String | Yes | 2026-02 | | Must follow YYYY-MM format |
| cost_category | Type of cost incurred | String | Yes | LABOR | | Allowed values only |
| amount | Posted cost amount | Decimal | Yes | 18500.00 | | Negative values allowed only for approved corrections or reversals |
| currency_code | Transaction currency | String | Yes | CAD | | Must be a valid three-letter currency code |
| transaction_type | Type of financial transaction | String | Yes | NORMAL | | Allowed values only |
| source_reference | Reference to the original source record | String | No | SAP-900145 | | Must not be empty when provided |
| invoice_id | Related invoice identifier | String | No | INV-000145 | FK | If provided, must exist in invoices.csv |
| created_at | Record creation timestamp | Datetime | Yes | 2026-02-18 13:45:00 | | Valid timestamp |

### Allowed transaction types

- NORMAL
- REVERSAL
- ADJUSTMENT

### Initial business rules for actual costs

- Every actual cost record must belong to an existing project and work package.
- The work package must belong to the referenced project.
- Negative values are allowed only for REVERSAL or approved ADJUSTMENT records.
- Posting date cannot be earlier than transaction date.
- Each financial period must exist in calendar.csv.
- The cost category must belong to the approved cost-category list.
- Actual cost records must not be duplicated using the same source reference and amount.

## 5. commitments.csv

**Granularity:** One row per commitment or commitment line.

| Column | Description | Type | Required | Example | Key | Validation |
|---|---|---|---|---|---|---|
| commitment_id | Unique commitment record identifier | String | Yes | COM-000001 | PK | Unique and not null |
| project_id | Project identifier | String | Yes | PRJ-001 | FK | Must exist in projects.csv |
| work_package_id | Work package identifier | String | Yes | WP-001-03 | FK | Must exist in work_packages.csv and belong to the same project |
| supplier_name | Supplier or subconsultant name | String | Yes | North Engineering Ltd. | | Not empty |
| commitment_type | Type of commitment | String | Yes | PURCHASE_ORDER | | Allowed values only |
| reference_number | External commitment reference | String | Yes | PO-4500123 | | Must not be empty |
| commitment_date | Date when the commitment was approved or created | Date | Yes | 2026-03-01 | | Valid date |
| planned_end_date | Expected completion date of the commitment | Date | No | 2026-11-30 | | Must not be before commitment_date |
| original_amount | Original approved commitment amount | Decimal | Yes | 350000.00 | | Must be greater than or equal to zero |
| current_commitment_amount | Current approved value after changes | Decimal | Yes | 375000.00 | | Must be greater than or equal to zero |
| actualized_amount | Amount already converted into actual cost | Decimal | Yes | 125000.00 | | Must be greater than or equal to zero |
| remaining_commitment | Remaining committed value | Decimal | Yes | 250000.00 | | Must be greater than or equal to zero |
| currency_code | Commitment currency | String | Yes | CAD | | Valid three-letter currency code |
| commitment_status | Current commitment status | String | Yes | OPEN | | Allowed values only |
| created_at | Record creation timestamp | Datetime | Yes | 2026-03-01 14:20:00 | | Valid timestamp |

### Allowed commitment types

- PURCHASE_ORDER
- SUBCONSULTANT_CONTRACT
- SERVICE_AGREEMENT
- OTHER

### Allowed commitment statuses

- OPEN
- PARTIALLY_ACTUALIZED
- FULLY_ACTUALIZED
- CLOSED
- CANCELLED

### Initial business rules for commitments

- Every commitment must belong to an existing project and work package.
- The work package must belong to the referenced project.
- current_commitment_amount cannot be negative.
- actualized_amount cannot exceed current_commitment_amount.
- remaining_commitment should equal current_commitment_amount minus actualized_amount.
- Cancelled commitments must have no new actualized amounts after cancellation.
- Historical changes to commitment values should remain traceable.

## 6. forecasts.csv

**Granularity:** One row per forecast version, project, work package, forecast period, and cost category.

| Column | Description | Type | Required | Example | Key | Validation |
|---|---|---|---|---|---|---|
| forecast_id | Unique forecast record identifier | String | Yes | FCT-000001 | PK | Unique and not null |
| forecast_version | Forecast reporting version | String | Yes | FCST_2026_03 | | Must not be empty |
| reporting_date | Date when the forecast version was issued | Date | Yes | 2026-03-31 | | Valid date |
| project_id | Project identifier | String | Yes | PRJ-001 | FK | Must exist in projects.csv |
| work_package_id | Work package identifier | String | Yes | WP-001-01 | FK | Must exist in work_packages.csv |
| forecast_period | Future period being forecasted | String | Yes | 2026-06 | | Must follow YYYY-MM format |
| cost_category | Forecasted cost category | String | Yes | LABOR | | Allowed values only |
| forecast_amount | Forecasted cost amount | Decimal | Yes | 98000.00 | | Must be greater than or equal to zero |
| forecast_hours | Forecasted labor hours | Decimal | No | 720.0 | | Must be greater than or equal to zero |
| currency_code | Forecast currency | String | Yes | CAD | | Must match project currency in first version |
| created_at | Record creation timestamp | Datetime | Yes | 2026-03-31 18:00:00 | | Valid timestamp |

### Initial business rules for forecasts

- Previous forecast versions must never be overwritten.
- Each forecast record must belong to an existing project and work package.
- The work package must belong to the referenced project.
- Forecast amounts and hours cannot be negative.
- The same combination of forecast version, work package, period, and cost category must not be duplicated.
- Forecast periods should normally be equal to or later than the reporting period.
- Historical forecast versions must remain available for variance analysis.

## 7. resources.csv

**Granularity:** One row per resource.

| Column | Description | Type | Required | Example | Key | Validation |
|---|---|---|---|---|---|---|
| resource_id | Unique resource identifier | String | Yes | RES-0001 | PK | Unique and not null |
| resource_name | Resource name | String | Yes | Emma Johnson | | Not empty |
| role_name | Job role or position | String | Yes | Civil Engineer | | Not empty |
| discipline | Main technical discipline | String | Yes | Civil | | Allowed category |
| employment_type | Type of resource engagement | String | Yes | EMPLOYEE | | Allowed values only |
| hourly_rate | Standard hourly cost rate | Decimal | Yes | 95.00 | | Must be greater than or equal to zero |
| currency_code | Rate currency | String | Yes | CAD | | Valid three-letter currency code |
| active_from | Date when the resource became active | Date | Yes | 2025-08-01 | | Valid date |
| active_to | Date when the resource became inactive | Date | No | 2027-12-31 | | Must be after active_from when provided |
| resource_status | Current resource status | String | Yes | ACTIVE | | Allowed values only |
| created_at | Record creation timestamp | Datetime | Yes | 2025-08-01 08:30:00 | | Valid timestamp |

### Allowed employment types

- EMPLOYEE
- CONTRACTOR
- SUBCONSULTANT

### Allowed resource statuses

- ACTIVE
- INACTIVE

### Initial business rules for resources

- Every resource must have a unique identifier.
- Hourly rates cannot be negative.
- active_to cannot be earlier than active_from.
- An inactive resource cannot register new timesheet records after the active_to date.
- Resource currency must be defined.

## 8. timesheets.csv

**Granularity:** One row per resource, work date, project, and work package.

| Column | Description | Type | Required | Example | Key | Validation |
|---|---|---|---|---|---|---|
| timesheet_id | Unique timesheet record identifier | String | Yes | TSH-000001 | PK | Unique and not null |
| resource_id | Resource identifier | String | Yes | RES-0001 | FK | Must exist in resources.csv |
| project_id | Project identifier | String | Yes | PRJ-001 | FK | Must exist in projects.csv |
| work_package_id | Work package identifier | String | Yes | WP-001-01 | FK | Must exist in work_packages.csv |
| work_date | Date when the work was performed | Date | Yes | 2026-03-12 | | Valid date |
| financial_period | Financial period associated with the work date | String | Yes | 2026-03 | | Must exist in calendar.csv |
| hours_worked | Number of hours worked | Decimal | Yes | 8.0 | | Must be greater than or equal to zero |
| hourly_rate | Applied hourly rate | Decimal | Yes | 95.00 | | Must be greater than or equal to zero |
| labor_cost | Calculated labor cost | Decimal | Yes | 760.00 | | Must equal hours_worked multiplied by hourly_rate |
| approval_status | Timesheet approval status | String | Yes | APPROVED | | Allowed values only |
| created_at | Record creation timestamp | Datetime | Yes | 2026-03-13 07:45:00 | | Valid timestamp |

### Allowed approval statuses

- SUBMITTED
- APPROVED
- REJECTED

### Initial business rules for timesheets

- Every timesheet record must reference an existing resource, project, and work package.
- The work package must belong to the referenced project.
- The resource must be active on the work date.
- Hours worked cannot be negative.
- labor_cost should equal hours_worked multiplied by hourly_rate.
- Approved timesheets may feed actual labor costs.
- Rejected timesheets must not contribute to actual project cost.

## 9. invoices.csv

**Granularity:** One row per invoice line.

| Column | Description | Type | Required | Example | Key | Validation |
|---|---|---|---|---|---|---|
| invoice_line_id | Unique invoice-line identifier | String | Yes | INVL-000001 | PK | Unique and not null |
| invoice_id | Supplier invoice identifier | String | Yes | INV-2026-0045 | | Must be unique by supplier at invoice level |
| supplier_name | Supplier or subconsultant name | String | Yes | North Engineering Ltd. | | Not empty |
| project_id | Project identifier | String | Yes | PRJ-001 | FK | Must exist in projects.csv |
| work_package_id | Work package identifier | String | Yes | WP-001-03 | FK | Must exist in work_packages.csv |
| commitment_id | Related commitment identifier | String | No | COM-000001 | FK | If provided, must exist in commitments.csv |
| invoice_date | Date shown on the invoice | Date | Yes | 2026-04-05 | | Valid date |
| posting_date | Date when the invoice was posted | Date | Yes | 2026-04-10 | | Must not be earlier than invoice_date |
| financial_period | Financial period assigned to the invoice | String | Yes | 2026-04 | | Must exist in calendar.csv |
| cost_category | Invoice cost category | String | Yes | SUBCONSULTANT | | Allowed values only |
| line_description | Description of invoice line | String | Yes | Detailed design services | | Not empty |
| gross_amount | Invoice line gross amount | Decimal | Yes | 42500.00 | | Must be greater than or equal to zero |
| currency_code | Invoice currency | String | Yes | CAD | | Valid currency code |
| invoice_status | Current invoice status | String | Yes | APPROVED | | Allowed values only |
| created_at | Record creation timestamp | Datetime | Yes | 2026-04-10 12:00:00 | | Valid timestamp |

### Allowed invoice statuses

- RECEIVED
- UNDER_REVIEW
- APPROVED
- POSTED
- PAID
- REJECTED

### Initial business rules for invoices

- Invoice identifiers must be unique for each supplier.
- Every invoice line must reference an existing project and work package.
- The work package must belong to the referenced project.
- If commitment_id is provided, the commitment must belong to the same project and work package.
- Posting date cannot be earlier than invoice date.
- Approved or posted invoices may generate actual costs.
- Rejected invoices must not contribute to project actual cost.

## 10. calendar.csv

**Granularity:** One row per calendar date.

| Column | Description | Type | Required | Example | Key | Validation |
|---|---|---|---|---|---|---|
| calendar_date | Calendar date | Date | Yes | 2026-03-31 | PK | Unique and not null |
| calendar_year | Calendar year | Integer | Yes | 2026 | | Must match calendar_date |
| calendar_month | Calendar month number | Integer | Yes | 3 | | Must be between 1 and 12 |
| month_name | Calendar month name | String | Yes | March | | Must match calendar_date |
| quarter | Calendar quarter | String | Yes | Q1 | | Allowed values Q1-Q4 |
| financial_period | Financial reporting period | String | Yes | 2026-03 | | Must follow YYYY-MM format |
| period_start_date | First date of the financial period | Date | Yes | 2026-03-01 | | Must be valid |
| period_end_date | Last date of the financial period | Date | Yes | 2026-03-31 | | Must be equal to or after period_start_date |
| is_month_end | Indicates whether the date is the final day of the month | Boolean | Yes | TRUE | | TRUE or FALSE |
| is_weekend | Indicates whether the date is Saturday or Sunday | Boolean | Yes | FALSE | | TRUE or FALSE |
| reporting_cutoff | Indicates whether the date is an official reporting cut-off | Boolean | Yes | TRUE | | TRUE or FALSE |

### Initial business rules for calendar data

- Every calendar date must be unique.
- Calendar year, month, quarter, and period must be consistent with calendar_date.
- Every transaction period referenced by another dataset must exist in calendar.csv.
- Reporting cut-off dates will be used to identify valid forecast versions and reporting snapshots.
