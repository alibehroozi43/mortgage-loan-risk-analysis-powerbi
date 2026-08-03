# Data Model

This document describes the semantic model used in the Mortgage Loan Risk Analysis Power BI project.

## Modeling Approach

The project uses a star schema.

The central fact table is:

```text
FactApplications
```

The surrounding dimension tables are:

```text
DimApplicants
DimProperties
DimProducts
DimUnderwriters
DimDate
```

The star-schema approach improves:

- Model clarity
- Filter behavior
- DAX performance
- Measure reuse
- Report maintainability
- Business interpretation

## Model Overview

```text
                    DimApplicants
                          |
                          |
DimProperties — FactApplications — DimProducts
                          |
                          |
                  DimUnderwriters
                          |
                          |
                       DimDate
```

`FactApplications` stores transactional mortgage application records.

Each dimension table provides descriptive information used to analyze the applications.

## Fact Table

### FactApplications

The fact table contains one row for each mortgage application.

Important fields include:

```text
ApplicationID
ApplicantID
PropertyID
ProductID
UnderwriterID
ApplicationDate
LoanAmount
Status
LTV
LTV Band
Risk Level
```

The fact table is located at the center of the model.

## Dimension Tables

### DimApplicants

Contains applicant information and merged credit-history attributes.

Important fields include:

```text
ApplicantID
FullName
DateOfBirth
Email
CreditScore
Bankruptcy
LatePayments
Credit Band
```

### DimProperties

Contains information about the property associated with each mortgage application.

Important fields include:

```text
PropertyID
Address
City
State
ZipCode
AppraisedValue
```

### DimProducts

Contains information about mortgage products.

Important fields include:

```text
ProductID
ProductName
InterestRate
TermMonths
```

### DimUnderwriters

Contains information about the employees responsible for reviewing mortgage applications.

Important fields include:

```text
UnderwriterID
Name
Department
```

### DimDate

Contains calendar attributes used for time-based analysis.

Important fields include:

```text
Date
Year
Quarter
Month
MonthNumber
Year-Month
```

## Relationships

The model uses the following relationships:

| From table and field | To table and field | Cardinality | Filter direction |
|---|---|---|---|
| FactApplications[ApplicantID] | DimApplicants[ApplicantID] | Many-to-One | Single |
| FactApplications[PropertyID] | DimProperties[PropertyID] | Many-to-One | Single |
| FactApplications[ProductID] | DimProducts[ProductID] | Many-to-One | Single |
| FactApplications[UnderwriterID] | DimUnderwriters[UnderwriterID] | Many-to-One | Single |
| FactApplications[ApplicationDate] | DimDate[Date] | Many-to-One | Single |

## Cardinality

Each dimension table has one unique record for every dimension key.

The fact table may contain multiple rows referencing the same applicant, property, product, underwriter, or date.

Therefore, the relationships are configured as:

```text
Many-to-One
```

The many side is always:

```text
FactApplications
```

The one side is the related dimension table.

## Filter Direction

All relationships use:

```text
Single-direction filtering
```

Filters flow from the dimension tables to the fact table.

For example:

```text
DimProducts → FactApplications
```

This allows users to select a mortgage product and filter the application records associated with that product.

Single-direction filtering is preferred because it:

- Reduces ambiguity
- Prevents unexpected filter propagation
- Simplifies DAX calculations
- Supports a clean star schema
- Improves model performance

## Date Relationship

The date relationship connects:

```text
DimDate[Date]
```

to:

```text
FactApplications[ApplicationDate]
```

This relationship supports:

- Monthly application trends
- Yearly comparisons
- Quarterly analysis
- Year-Month reporting
- Time-based slicers

The `DimDate` table should be marked as the official date table in Power BI.

## Date Table

The date table was created using DAX:

```DAX
DimDate =
VAR MinDate =
    MIN ( FactApplications[ApplicationDate] )

VAR MaxDate =
    MAX ( FactApplications[ApplicationDate] )

RETURN
ADDCOLUMNS (
    CALENDAR ( MinDate, MaxDate ),
    "Year", YEAR ( [Date] ),
    "Quarter", "Q" & QUARTER ( [Date] ),
    "Month", FORMAT ( [Date], "MMMM" ),
    "MonthNumber", MONTH ( [Date] ),
    "Year-Month", FORMAT ( [Date], "YYYY-MM" )
)
```

After creating the table:

1. Select the `Month` column.
2. Use `Sort by column`.
3. Select `MonthNumber`.
4. Mark `DimDate` as the date table.
5. Select the `Date` column as the date field.

## Why a Flat Table Was Not Used

The project does not merge every source into one large table.

A single flat table would:

- Repeat applicant information
- Repeat product information
- Repeat underwriter information
- Increase model size
- Reduce maintainability
- Make relationships harder to understand
- Increase the risk of incorrect aggregation

The star-schema design separates transactions from descriptive attributes.

## Model Validation

The following checks were performed:

| Validation check | Result |
|---|---|
| Duplicate applicant keys | 0 |
| Duplicate property keys | 0 |
| Duplicate product keys | 0 |
| Duplicate underwriter keys | 0 |
| Null values in primary keys | 0 |
| Many-to-many relationships | None |
| Bidirectional relationships | None |
| Unnecessary independent credit-history table | Disabled after merge |

## Data Model Image

The Power BI data model is shown below:

![Power BI Data Model](../images/data_model.png)

## Modeling Best Practices Applied

This project applies the following Power BI modeling practices:

- A central fact table
- Separate dimension tables
- Unique dimension keys
- Many-to-one relationships
- Single-direction filtering
- A dedicated date table
- Measures separated from raw data columns
- Disabled loading for unnecessary staging queries
- Clear table naming conventions

## Conclusion

The data model provides a reliable foundation for analyzing:

- Mortgage application volume
- Approval and rejection rates
- Requested loan amounts
- Credit risk
- Loan-to-value ratios
- Mortgage products
- Underwriter performance
- Geographic application trends
- Time-based changes
