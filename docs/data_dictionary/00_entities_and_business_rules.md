# Entities and Business Rules

## 1. Purpose

This document defines the main business entities, relationships, data granularity, and initial business rules for the Project Control Analytics Platform.

The objective is to establish a consistent data structure before generating the synthetic datasets and building the ingestion pipeline.

## 2. Main Entities

### 2.1 Projects

Represents the projects monitored by the platform.

A project contains general information such as:

- Project identifier.
- Project name.
- Client.
- Start date.
- Planned completion date.
- Current status.
- Approved budget.
- Currency.
- Project manager.

### 2.2 Work Packages

Represents subdivisions of work within a project.

Each work package:

- Belongs to one project.
- Has a unique identifier within the platform.
- Has planned start and finish dates.
- May have its own approved budget.
- May generate actual costs, commitments, forecasts, hours, and invoices.

### 2.3 Budget

Represents the approved financial plan.

Budget records may be stored by:

- Project.
- Work package.
- Financial period.
- Cost category.

### 2.4 Actual Costs

Represents costs already incurred and posted.

Each actual-cost record must be associated with:

- A project.
- A financial period.
- A cost category.
- Optionally, a work package, invoice, resource, or supplier.

### 2.5 Commitments

Represents contractual or purchasing obligations that have not yet been fully converted into actual costs.

Commitments may include:

- Purchase orders.
- Subconsultant contracts.
- Approved service commitments.
- Remaining contractual values.

### 2.6 Forecasts

Represents the expected future costs of a project.

Forecasts must retain:

- Forecast version.
- Reporting date.
- Forecast period.
- Project and work package.
- Forecast amount.
- Forecast hours, when applicable.

### 2.7 Resources

Represents people or labor profiles assigned to project work.

Resources may include:

- Employee identifier.
- Role.
- Discipline.
- Hourly rate.
- Employment type.
- Active status.

### 2.8 Timesheets

Represents hours worked by resources.

Each timesheet record must identify:

- Resource.
- Project.
- Work package.
- Work date or financial period.
- Number of hours.
- Labor cost, when applicable.

### 2.9 Invoices

Represents invoices received from suppliers or subconsultants.

Each invoice may contain:

- Invoice identifier.
- Supplier.
- Project.
- Work package.
- Invoice date.
- Posting date.
- Gross amount.
- Paid or approved status.

### 2.10 Calendar

Represents the reporting and financial periods used throughout the platform.

It will support:

- Monthly reporting.
- Fiscal periods.
- Reporting cut-off dates.
- Year and quarter calculations.

## 3. Initial Relationships

- One project can contain many work packages.
- One work package belongs to one project.
- One project can have many budget records.
- One work package can have many actual-cost records.
- One work package can have many commitment records.
- One work package can have many forecast records.
- One resource can register hours in multiple work packages.
- One work package can receive hours from multiple resources.
- One invoice belongs to one project.
- One invoice may be associated with one or more work packages.
- Each financial transaction must belong to a valid calendar period.

## 4. Initial Business Rules

1. Every project must have a unique project identifier.
2. Every work package must belong to an existing project.
3. Financial amounts must use a defined currency.
4. Budget amounts cannot be negative.
5. Actual costs may be negative only for approved reversals or corrections.
6. Forecast versions must not overwrite previous versions.
7. Each forecast record must include a reporting date and forecast period.
8. Timesheet hours cannot be negative.
9. Timesheet hours must belong to an active project period.
10. Invoice identifiers must be unique by supplier.
11. A financial record cannot reference a nonexistent work package.
12. Project start dates must be earlier than planned completion dates.
13. Work-package dates must fall within the corresponding project period.
14. Closed projects cannot receive new forecast records unless explicitly reopened.
15. The synthetic dataset must include controlled data-quality issues for validation testing.

## 5. Data Granularity

| Dataset | Expected Granularity |
|---|---|
| Projects | One row per project |
| Work Packages | One row per work package |
| Budget | One row per project, work package, period, and cost category |
| Actual Costs | One row per financial transaction |
| Commitments | One row per commitment or purchase-order line |
| Forecasts | One row per forecast version, period, work package, and cost category |
| Resources | One row per resource |
| Timesheets | One row per resource, work date, and work package |
| Invoices | One row per invoice or invoice line |
| Calendar | One row per calendar date |

## 6. Pending Decisions

The following items will be defined during the detailed data-dictionary design:

- Cost categories.
- Project statuses.
- Work-package statuses.
- Currency handling.
- Forecast version naming.
- Invoice-to-work-package relationship.
- Calculation of committed cost.
- Treatment of reversals and adjustments.
- Financial-period cut-off logic.
