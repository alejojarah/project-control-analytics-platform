# CRISP-DM Phase 1: Business Understanding

## 1. Project Title

Project Control Analytics Platform

## 2. Project Overview

The Project Control Analytics Platform is an end-to-end data solution designed to consolidate, validate, analyze, and model financial and operational project data.

The platform will integrate project budgets, actual costs, commitments, forecasts, work packages, resources, timesheets, and invoices in order to provide reliable project performance indicators and early warnings of potential cost overruns.

## 3. Business Context

Organizations that manage complex projects often store financial and operational data across multiple spreadsheets, systems, and reporting tools.

This fragmented environment may result in:

- Manual consolidation processes.
- Duplicate or inconsistent records.
- Different naming conventions and identifiers.
- Limited traceability of forecast changes.
- Delayed identification of cost deviations.
- Difficulty estimating the final project cost.
- High dependence on manual calculations and spreadsheet formulas.

A centralized data platform can improve the reliability, speed, and transparency of project-control reporting.

## 4. Business Problem

Project controllers and project managers require a centralized and reliable solution that consolidates financial and operational information, evaluates data quality, calculates project-control indicators, and identifies potential cost overruns before they become critical.

## 5. Business Objective

Develop a data platform that improves project cost monitoring by:

- Automating the consolidation of project information.
- Standardizing and validating data from multiple sources.
- Calculating financial and operational performance indicators.
- Comparing actual performance against budgets and forecasts.
- Identifying projects and work packages with financial risk.
- Supporting project management decision-making.
- Estimating the final project cost.
- Predicting the probability of cost overruns.

## 6. Technical Objective

Build a reproducible end-to-end data solution that:

- Ingests data from CSV and Excel files.
- Stores raw and processed data in structured layers.
- Applies automated data-quality validations.
- Loads standardized information into PostgreSQL.
- Creates analytical data models using SQL.
- Generates data marts for reporting and machine learning.
- Provides dashboards for project monitoring.
- Trains predictive models for cost-overrun risk and final cost estimation.
- Documents the complete process using CRISP-DM.

## 7. Stakeholders and Users

| Stakeholder | Main Need |
|---|---|
| Project Controller | Consolidate data, validate records, calculate indicators, and prepare reports |
| Project Manager | Monitor the financial and operational status of the project |
| Work Package Manager | Review the performance and risk of assigned work packages |
| Finance Management | Analyze costs, forecasts, commitments, and projected results |
| Data Team | Maintain pipelines, data models, quality rules, and analytical products |

## 8. Business Questions

The platform should answer the following questions:

1. What is the current financial status of each project?
2. What is the current Estimate at Completion?
3. Which projects or work packages are expected to exceed their budget?
4. How has the forecast changed between reporting periods?
5. Which work packages have the largest cost deviations?
6. Which resources or disciplines are consuming more hours than planned?
7. What percentage of the approved budget has already been spent?
8. What percentage of the budget is already committed?
9. Which invoices or cost records contain inconsistencies?
10. What are the main drivers of cost variation?
11. What is the expected final cost of each project?
12. What is the probability that a project or work package will end with a cost overrun?

## 9. Initial Key Performance Indicators

The first version of the platform will calculate:

- Budget.
- Actual Cost.
- Committed Cost.
- Estimate to Complete.
- Estimate at Completion.
- Cost Variance.
- Forecast Variance.
- Remaining Budget.
- Budget Consumption.
- Committed Exposure.
- Monthly Burn Rate.
- Hours Variance.
- Forecast Change.
- Cost Overrun Risk.

## 10. Project Scope

### In Scope

- Synthetic project-control data.
- Multiple projects and work packages.
- Budgets and monthly forecasts.
- Actual and committed costs.
- Resource hours and timesheets.
- Invoice records.
- Data-quality validation.
- PostgreSQL database.
- Python ingestion pipeline.
- SQL transformations.
- Analytical data model.
- Dashboard.
- Classification model for cost-overrun risk.
- Regression model for final cost estimation.
- Documentation in GitHub.

### Out of Scope for the First Version

- Direct SAP integration.
- Real-time data processing.
- Enterprise cloud infrastructure.
- Mobile application.
- Complex user authentication.
- Generative AI features.
- Use of confidential company information.
- Full production-grade monitoring.

## 11. Business Success Criteria

The project will be considered successful from a business perspective if it:

- Consolidates all defined data sources.
- Detects incomplete, duplicated, or inconsistent records.
- Calculates reliable indicators by project and work package.
- Identifies financial risks before project completion.
- Reduces dependence on manual spreadsheet consolidation.
- Provides clear and actionable information for decision-making.

## 12. Technical Success Criteria

The project will be considered technically successful if it includes:

- A reproducible data pipeline.
- A documented PostgreSQL database.
- Automated data-quality rules.
- Modular Python code.
- SQL transformations and analytical marts.
- Basic unit tests.
- A functional dashboard.
- Machine learning models compared against simple baselines.
- Clear installation and execution instructions.
- Version control and structured commits in GitHub.

## 13. Portfolio Success Criteria

The project should demonstrate:

- Business understanding.
- Data engineering.
- SQL and data modeling.
- Data analysis and visualization.
- Machine learning.
- Software engineering practices.
- Documentation.
- Communication of technical and business results.

A recruiter should be able to understand the problem, architecture, methodology, and results without reviewing every source-code file.

## 14. Constraints

- The dataset will initially be synthetic.
- The project will be developed by one person.
- The first version will prioritize local execution.
- The predictive performance will depend on the realism and size of the synthetic dataset.
- The project must remain understandable and executable by external reviewers.

## 15. Main Risks

| Risk | Mitigation |
|---|---|
| Synthetic data is not sufficiently realistic | Define business rules before generating the data |
| Project scope becomes too large | Develop the platform in incremental versions |
| Machine learning adds little business value | Compare models against clear baselines |
| Architecture becomes unnecessarily complex | Add technologies only when they solve a specific need |
| Documentation becomes outdated | Update documentation during each development phase |
| Confidential information is exposed | Use only synthetic or properly anonymized data |

## 16. Initial Project Plan

1. Complete Business Understanding.
2. Design the source entities and business rules.
3. Define the data dictionary.
4. Generate the synthetic dataset.
5. Perform Data Understanding.
6. Build the ingestion and validation pipeline.
7. Create the PostgreSQL data model.
8. Develop analytical marts and KPIs.
9. Build the dashboard.
10. Train and evaluate predictive models.
11. Package and deploy the solution.
12. Complete the README and project presentation.
