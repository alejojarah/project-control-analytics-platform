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
